---
title: Balansera Azure Service Fabric-kluster | Microsoft Docs
description: En introduktion till NLB-klustret med Service Fabric klustret Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 34eacb29f324025c1d2803c9690600227d3ec457
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="balancing-your-service-fabric-cluster"></a><span data-ttu-id="c5902-103">NLB service fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="c5902-103">Balancing your service fabric cluster</span></span>
<span data-ttu-id="c5902-104">Service Fabric klustret Resource Manager stöder dynamisk ändringar reagera på tillägg eller borttagning av noder eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="c5902-104">The Service Fabric Cluster Resource Manager supports dynamic load changes, reacting to additions or removals of nodes or services.</span></span> <span data-ttu-id="c5902-105">Den korrigerar också automatiskt begränsningen överträdelser och balanserar proaktivt klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-105">It also automatically corrects constraint violations, and proactively rebalances the cluster.</span></span> <span data-ttu-id="c5902-106">Hur ofta dessa åtgärder vidtas för men vad utlöser dem?</span><span class="sxs-lookup"><span data-stu-id="c5902-106">But how often are these actions taken, and what triggers them?</span></span>

<span data-ttu-id="c5902-107">Det finns tre olika kategorier av arbete som utförs av klustret Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c5902-107">There are three different categories of work that the Cluster Resource Manager performs.</span></span> <span data-ttu-id="c5902-108">De är:</span><span class="sxs-lookup"><span data-stu-id="c5902-108">They are:</span></span>

1. <span data-ttu-id="c5902-109">Placering – det här steget behandlar placera alla tillståndskänslig repliker eller tillståndslösa instanser som saknas.</span><span class="sxs-lookup"><span data-stu-id="c5902-109">Placement – this stage deals with placing any stateful replicas or stateless instances that are missing.</span></span> <span data-ttu-id="c5902-110">Placering innehåller både nya tjänster och hanterar tillståndskänslig repliker eller tillståndslösa instanser som har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="c5902-110">Placement includes both new services and handling stateful replicas or stateless instances that have failed.</span></span> <span data-ttu-id="c5902-111">Ta bort och släppa repliker eller instanser hanteras här.</span><span class="sxs-lookup"><span data-stu-id="c5902-111">Deleting and dropping replicas or instances are handled here.</span></span>
2. <span data-ttu-id="c5902-112">Begränsningen kontrollerar – det här steget kontrollerar och korrigerar överträdelser av olika placeringen (regler) i systemet.</span><span class="sxs-lookup"><span data-stu-id="c5902-112">Constraint Checks – this stage checks for and corrects violations of the different placement constraints (rules) within the system.</span></span> <span data-ttu-id="c5902-113">Exempel på regler är till exempel att säkerställa att noderna inte är överbelastade och att en tjänst placeringen är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="c5902-113">Examples of rules are things like ensuring that nodes are not over capacity and that a service’s placement constraints are met.</span></span>
3. <span data-ttu-id="c5902-114">Belastningsutjämning – det här steget kontrollerar om det finns ombalansering nödvändiga baserat på konfigurerad önskad tjänstnivå balansera för olika mått.</span><span class="sxs-lookup"><span data-stu-id="c5902-114">Balancing – this stage checks to see if rebalancing is necessary based on the configured desired level of balance for different metrics.</span></span> <span data-ttu-id="c5902-115">I så fall försök görs att hitta en ordning i klustret som är mer balanserad.</span><span class="sxs-lookup"><span data-stu-id="c5902-115">If so it attempts to find an arrangement in the cluster that is more balanced.</span></span>

## <a name="configuring-cluster-resource-manager-timers"></a><span data-ttu-id="c5902-116">Konfigurera Timers för hanteraren för filserverresurser</span><span class="sxs-lookup"><span data-stu-id="c5902-116">Configuring Cluster Resource Manager Timers</span></span>
<span data-ttu-id="c5902-117">Den första uppsättningen kontroller runt belastningsutjämning är en uppsättning timers.</span><span class="sxs-lookup"><span data-stu-id="c5902-117">The first set of controls around balancing are a set of timers.</span></span> <span data-ttu-id="c5902-118">Dessa timers styr hur ofta klustret Resource Manager undersöker klustret och tar korrigerande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c5902-118">These timers govern how often the Cluster Resource Manager examines the cluster and takes corrective actions.</span></span>

<span data-ttu-id="c5902-119">Var och en av dessa olika typer av klustret Resource Manager kan göra korrigeringar styrs av en annan timer som styr frekvensen.</span><span class="sxs-lookup"><span data-stu-id="c5902-119">Each of these different types of corrections the Cluster Resource Manager can make is controlled by a different timer that governs its frequency.</span></span> <span data-ttu-id="c5902-120">När varje timer utlöses har uppgiften schemalagts.</span><span class="sxs-lookup"><span data-stu-id="c5902-120">When each timer fires, the task is scheduled.</span></span> <span data-ttu-id="c5902-121">Som standard Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="c5902-121">By default the Resource Manager:</span></span>

* <span data-ttu-id="c5902-122">genomsöker dess tillstånd och tillämpar uppdateringar (till exempel inspelningen som en nod nedåt) var 1/10 sekunder</span><span class="sxs-lookup"><span data-stu-id="c5902-122">scans its state and applies updates (like recording that a node is down) every 1/10th of a second</span></span>
* <span data-ttu-id="c5902-123">Anger att kontrollera placering</span><span class="sxs-lookup"><span data-stu-id="c5902-123">sets the placement check flag</span></span> 
* <span data-ttu-id="c5902-124">Anger att begränsningen kontrollera varje sekund</span><span class="sxs-lookup"><span data-stu-id="c5902-124">sets the constraint check flag every second</span></span>
* <span data-ttu-id="c5902-125">ställer in flaggan belastningsutjämning var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="c5902-125">sets the balancing flag every five seconds.</span></span>

<span data-ttu-id="c5902-126">Exempel på konfigurationen för de här räknarna är nedan:</span><span class="sxs-lookup"><span data-stu-id="c5902-126">Examples of the configuration governing these timers are below:</span></span>

<span data-ttu-id="c5902-127">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="c5902-127">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

<span data-ttu-id="c5902-128">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="c5902-128">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

<span data-ttu-id="c5902-129">Idag utför klustret Resource Manager endast en av dessa åtgärder samtidigt, sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="c5902-129">Today the Cluster Resource Manager only performs one of these actions at a time, sequentially.</span></span> <span data-ttu-id="c5902-130">Det är därför vi refererar till dessa timers som ”minsta intervall” och de åtgärder som får vidtas när timers går som ”inställningen flaggor”.</span><span class="sxs-lookup"><span data-stu-id="c5902-130">This is why we refer to these timers as “minimum intervals” and the actions that get taken when the timers go off as "setting flags".</span></span> <span data-ttu-id="c5902-131">Till exempel sköter klustret Resource Manager väntande begäranden om att skapa tjänster innan NLB-klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-131">For example, the Cluster Resource Manager takes care of pending requests to create services before balancing the cluster.</span></span> <span data-ttu-id="c5902-132">Som du ser standard tidsintervall som angetts skannar Cluster Resource Manager för allt som behövs för att göra ofta.</span><span class="sxs-lookup"><span data-stu-id="c5902-132">As you can see by the default time intervals specified, the Cluster Resource Manager scans for anything it needs to do frequently.</span></span> <span data-ttu-id="c5902-133">Detta innebär normalt att uppsättning ändringar som gjorts under varje steg är liten.</span><span class="sxs-lookup"><span data-stu-id="c5902-133">Normally this means that the set of changes made during each step is small.</span></span> <span data-ttu-id="c5902-134">Ändrar mindre ofta kan klustret Resource Manager kunna svara när saker i klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-134">Making small changes frequently allows the Cluster Resource Manager to be responsive when things happen in the cluster.</span></span> <span data-ttu-id="c5902-135">Standard-timers ange vissa batchbearbetning eftersom många av samma typ av händelser tenderar att ske samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c5902-135">The default timers provide some batching since many of the same types of events tend to occur simultaneously.</span></span> 

<span data-ttu-id="c5902-136">Till exempel när noder misslyckas de kan göra så att hela feldomäner i taget.</span><span class="sxs-lookup"><span data-stu-id="c5902-136">For example, when nodes fail they can do so entire fault domains at a time.</span></span> <span data-ttu-id="c5902-137">Alla dessa fel avbildas under nästa tillstånd uppdatera efter den *PLBRefreshGap*.</span><span class="sxs-lookup"><span data-stu-id="c5902-137">All these failures are captured during the next state update after the *PLBRefreshGap*.</span></span> <span data-ttu-id="c5902-138">Korrigeringarna som fastställs under placeringen begränsningen kontroll och belastningsutjämning körs.</span><span class="sxs-lookup"><span data-stu-id="c5902-138">The corrections are determined during the following placement, constraint check, and balancing runs.</span></span> <span data-ttu-id="c5902-139">Som standard klustret Resource Manager inte genomsöka via timmar för ändringar i klustret och försök att åtgärda alla ändringar på en gång.</span><span class="sxs-lookup"><span data-stu-id="c5902-139">By default the Cluster Resource Manager is not scanning through hours of changes in the cluster and trying to address all changes at once.</span></span> <span data-ttu-id="c5902-140">Detta leder till belastning av omsättning.</span><span class="sxs-lookup"><span data-stu-id="c5902-140">Doing so would lead to bursts of churn.</span></span>

<span data-ttu-id="c5902-141">Resurshanteraren för klustret måste också ytterligare information för att avgöra om klustret imbalanced.</span><span class="sxs-lookup"><span data-stu-id="c5902-141">The Cluster Resource Manager also needs some additional information to determine if the cluster imbalanced.</span></span> <span data-ttu-id="c5902-142">Som vi har två delar i konfigurationen: *BalancingThresholds* och *ActivityThresholds*.</span><span class="sxs-lookup"><span data-stu-id="c5902-142">For that we have two other pieces of configuration: *BalancingThresholds* and *ActivityThresholds*.</span></span>

## <a name="balancing-thresholds"></a><span data-ttu-id="c5902-143">NLB tröskelvärden</span><span class="sxs-lookup"><span data-stu-id="c5902-143">Balancing thresholds</span></span>
<span data-ttu-id="c5902-144">Ett tröskelvärde för belastningsutjämning är den viktigaste kontrollen för att utlösa ombalansering.</span><span class="sxs-lookup"><span data-stu-id="c5902-144">A Balancing Threshold is the main control for triggering rebalancing.</span></span> <span data-ttu-id="c5902-145">NLB tröskelvärdet för ett mått är ett _förhållandet_.</span><span class="sxs-lookup"><span data-stu-id="c5902-145">The Balancing Threshold for a metric is a _ratio_.</span></span> <span data-ttu-id="c5902-146">Om belastningen för ett mått på noden mest inlästa dividerat med mängden belastningen på noden minst inlästa överskrider den måtten *BalancingThreshold*, då är imbalanced klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-146">If the load for a metric on the most loaded node divided by the amount of load on the least loaded node exceeds that metric's *BalancingThreshold*, then the cluster is imbalanced.</span></span> <span data-ttu-id="c5902-147">Som ett resultat av nätverksbelastning utlöses nästa gång klustret Resource Manager kontrollerar.</span><span class="sxs-lookup"><span data-stu-id="c5902-147">As a result balancing is triggered the next time the Cluster Resource Manager checks.</span></span> <span data-ttu-id="c5902-148">Den *MinLoadBalancingInterval* timer definierar hur ofta klustret Resource Manager ska kontrollera om ombalansering krävs.</span><span class="sxs-lookup"><span data-stu-id="c5902-148">The *MinLoadBalancingInterval* timer defines how often the Cluster Resource Manager should check if rebalancing is necessary.</span></span> <span data-ttu-id="c5902-149">Kontrollera innebär inte att något händer.</span><span class="sxs-lookup"><span data-stu-id="c5902-149">Checking doesn't mean that anything happens.</span></span> 

<span data-ttu-id="c5902-150">Tröskelvärden för belastningsutjämning definieras på grundval av per mått som en del av definitionen för klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-150">Balancing Thresholds are defined on a per-metric basis as a part of the cluster definition.</span></span> <span data-ttu-id="c5902-151">Mer information om mått kolla [i den här artikeln](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="c5902-151">For more information on metrics, check out [this article](service-fabric-cluster-resource-manager-metrics.md).</span></span>

<span data-ttu-id="c5902-152">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="c5902-152">ClusterManifest.xml</span></span>

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

<span data-ttu-id="c5902-153">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="c5902-153">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<span data-ttu-id="c5902-154"><center>
![Belastningsutjämning tröskelvärde-exempel][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="c5902-154"><center>
![Balancing Threshold Example][Image1]
</center></span></span>

<span data-ttu-id="c5902-155">I det här exemplet förbrukar en enhet av vissa mått för varje tjänst.</span><span class="sxs-lookup"><span data-stu-id="c5902-155">In this example, each service is consuming one unit of some metric.</span></span> <span data-ttu-id="c5902-156">I exemplet högsta maximala belastningen på en nod är fem och minst är två.</span><span class="sxs-lookup"><span data-stu-id="c5902-156">In the top example, the maximum load on a node is five and the minimum is two.</span></span> <span data-ttu-id="c5902-157">Anta att tröskelvärdet för belastningsutjämning för det här måttet är tre.</span><span class="sxs-lookup"><span data-stu-id="c5902-157">Let’s say that the balancing threshold for this metric is three.</span></span> <span data-ttu-id="c5902-158">Eftersom förhållandet i klustret är 5/2 = 2,5 och som är mindre än det angivna tröskelvärdet på tre, klustret är belastningsutjämnat.</span><span class="sxs-lookup"><span data-stu-id="c5902-158">Since the ratio in the cluster is 5/2 = 2.5 and that is less than the specified balancing threshold of three, the cluster is balanced.</span></span> <span data-ttu-id="c5902-159">Ingen belastningsutjämning utlöses när klustret Resource Manager kontrollerar.</span><span class="sxs-lookup"><span data-stu-id="c5902-159">No balancing is triggered when the Cluster Resource Manager checks.</span></span>

<span data-ttu-id="c5902-160">I exemplet längst ned är maximala belastningen på en nod 10, medan minst två, vilket resulterar i ett förhållande på fem.</span><span class="sxs-lookup"><span data-stu-id="c5902-160">In the bottom example, the maximum load on a node is 10, while the minimum is two, resulting in a ratio of five.</span></span> <span data-ttu-id="c5902-161">Fem är större än tröskelvärdet för avsedda belastningsutjämning på tre för att mått.</span><span class="sxs-lookup"><span data-stu-id="c5902-161">Five is greater than the designated balancing threshold of three for that metric.</span></span> <span data-ttu-id="c5902-162">Detta innebär är en ombalansering kör schemalagda nästa gång belastningsutjämning timern utlöses.</span><span class="sxs-lookup"><span data-stu-id="c5902-162">As a result, a rebalancing run will be scheduled next time the balancing timer fires.</span></span> <span data-ttu-id="c5902-163">I den här situationen är vanligtvis vissa belastningen distribueras till Nod3.</span><span class="sxs-lookup"><span data-stu-id="c5902-163">In a situation like this some load is usually distributed to Node3.</span></span> <span data-ttu-id="c5902-164">Eftersom Service Fabric klustret Resource Manager inte använder en girig metod, kan även vissa belastningen distribueras till nod 2.</span><span class="sxs-lookup"><span data-stu-id="c5902-164">Because the Service Fabric Cluster Resource Manager doesn't use a greedy approach, some load could also be distributed to Node2.</span></span> 

<span data-ttu-id="c5902-165"><center>
![Belastningsutjämning tröskelvärdet exempel åtgärder][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="c5902-165"><center>
![Balancing Threshold Example Actions][Image2]
</center></span></span>

> [!NOTE]
> <span data-ttu-id="c5902-166">”NLB” hanterar två olika strategier för att hantera belastningen i klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-166">"Balancing" handles two different strategies for managing load in your cluster.</span></span> <span data-ttu-id="c5902-167">Standard-strategin som klustret Resource Manager använder är att fördela belastningen över noderna i klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-167">The default strategy that the Cluster Resource Manager uses is to distribute load across the nodes in the cluster.</span></span> <span data-ttu-id="c5902-168">Strategin som helst är [defragmentering](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="c5902-168">The other strategy is [defragmentation](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span></span> <span data-ttu-id="c5902-169">Defragmentering utförs under samma fördelningen kör.</span><span class="sxs-lookup"><span data-stu-id="c5902-169">Defragmentation is performed during the same balancing run.</span></span> <span data-ttu-id="c5902-170">Belastningsutjämning och defragmentering strategier kan användas för olika mått i samma kluster.</span><span class="sxs-lookup"><span data-stu-id="c5902-170">The balancing and defragmentation strategies can be used for different metrics within the same cluster.</span></span> <span data-ttu-id="c5902-171">En tjänst kan ha belastningsutjämning och defragmentering mått.</span><span class="sxs-lookup"><span data-stu-id="c5902-171">A service can have both balancing and defragmentation metrics.</span></span> <span data-ttu-id="c5902-172">Förhållandet mellan belastningar i klustret för defragmentering mått utlöser ombalansering när det är _nedan_ tröskelvärdet för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="c5902-172">For defragmentation metrics, the ratio of the loads in the cluster triggers rebalancing when it is _below_ the balancing threshold.</span></span> 
>

<span data-ttu-id="c5902-173">Hämta under tröskelvärdet för belastningsutjämning är inte en explicit målet.</span><span class="sxs-lookup"><span data-stu-id="c5902-173">Getting below the balancing threshold is not an explicit goal.</span></span> <span data-ttu-id="c5902-174">Tröskelvärden för belastningsutjämning är bara en *utlösaren*.</span><span class="sxs-lookup"><span data-stu-id="c5902-174">Balancing Thresholds are just a *trigger*.</span></span> <span data-ttu-id="c5902-175">När NLB körs avgör klustret Resource Manager vilka förbättringar som det kan göra eventuella.</span><span class="sxs-lookup"><span data-stu-id="c5902-175">When balancing runs, the Cluster Resource Manager determines what improvements it can make, if any.</span></span> <span data-ttu-id="c5902-176">Bara för att en belastningsutjämning sökning har inletts innebär inte något flyttas.</span><span class="sxs-lookup"><span data-stu-id="c5902-176">Just because a balancing search is kicked off doesn't mean anything moves.</span></span> <span data-ttu-id="c5902-177">Ibland är klustringen imbalanced men begränsad för att åtgärda.</span><span class="sxs-lookup"><span data-stu-id="c5902-177">Sometimes the cluster is imbalanced but too constrained to correct.</span></span> <span data-ttu-id="c5902-178">Alternativt förbättringar kräver förflyttningar för [kostsamma](service-fabric-cluster-resource-manager-movement-cost.md)).</span><span class="sxs-lookup"><span data-stu-id="c5902-178">Alternatively, the improvements require movements that are too [costly](service-fabric-cluster-resource-manager-movement-cost.md)).</span></span>

## <a name="activity-thresholds"></a><span data-ttu-id="c5902-179">Tröskelvärden för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="c5902-179">Activity thresholds</span></span>
<span data-ttu-id="c5902-180">Ibland, men noder är relativt imbalanced den *totala* belastningen i klustret är låg.</span><span class="sxs-lookup"><span data-stu-id="c5902-180">Sometimes, although nodes are relatively imbalanced, the *total* amount of load in the cluster is low.</span></span> <span data-ttu-id="c5902-181">Bristen på belastning kan vara tillfälliga dip eller eftersom klustret är ny och bara hämtning startade.</span><span class="sxs-lookup"><span data-stu-id="c5902-181">The lack of load could be a transient dip, or because the cluster is new and just getting bootstrapped.</span></span> <span data-ttu-id="c5902-182">I båda fallen kan du inte vill ägna tid NLB klustret eftersom det är onödigt att.</span><span class="sxs-lookup"><span data-stu-id="c5902-182">In either case, you may not want to spend time balancing the cluster because there’s little to be gained.</span></span> <span data-ttu-id="c5902-183">Om klustret genomgått belastningsutjämning, du ägnar åt nätverket och beräkna resurser för att flytta runt objekt utan att göra några stora *absolut* skillnaden.</span><span class="sxs-lookup"><span data-stu-id="c5902-183">If the cluster underwent balancing, you’d spend network and compute resources to move things around without making any large *absolute* difference.</span></span> <span data-ttu-id="c5902-184">För att undvika flyttar onödiga, det finns en annan kontroll kallas tröskelvärden för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c5902-184">To avoid unnecessary moves, there’s another control known as Activity Thresholds.</span></span> <span data-ttu-id="c5902-185">Aktiviteten tröskelvärden kan du ange en absolut nedre gräns för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c5902-185">Activity Thresholds allows you to specify some absolute lower bound for activity.</span></span> <span data-ttu-id="c5902-186">Om ingen nod är över tröskeln, är inte belastningsutjämning utlösas även om tröskelvärdet för NLB är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="c5902-186">If no node is over this threshold, balancing isn't triggered even if the Balancing Threshold is met.</span></span>

<span data-ttu-id="c5902-187">Anta att vi behåller våra tröskelvärdet för belastningsutjämning av tre för det här måttet.</span><span class="sxs-lookup"><span data-stu-id="c5902-187">Let’s say that we retain our Balancing Threshold of three for this metric.</span></span> <span data-ttu-id="c5902-188">Anta också att vi har ett tröskelvärde för aktiviteten på 1536.</span><span class="sxs-lookup"><span data-stu-id="c5902-188">Let's also say we have an Activity Threshold of 1536.</span></span> <span data-ttu-id="c5902-189">I det första fallet när klustret är imbalanced per NLB tröskelvärdet som det finns uppfyller ingen nod att tröskelvärdet för aktiviteten så händer ingenting.</span><span class="sxs-lookup"><span data-stu-id="c5902-189">In the first case, while the cluster is imbalanced per the Balancing Threshold there's no node meets that Activity Threshold, so nothing happens.</span></span> <span data-ttu-id="c5902-190">I exemplet längst ned är Nod1 över tröskelvärdet för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c5902-190">In the bottom example, Node1 is over the Activity Threshold.</span></span> <span data-ttu-id="c5902-191">NLB har schemalagts eftersom både tröskelvärdet för belastningsutjämning och aktivitet tröskelvärdet för måttet överskrids.</span><span class="sxs-lookup"><span data-stu-id="c5902-191">Since both the Balancing Threshold and the Activity Threshold for the metric are exceeded, balancing is scheduled.</span></span> <span data-ttu-id="c5902-192">Exempelvis ska vi titta på följande diagram:</span><span class="sxs-lookup"><span data-stu-id="c5902-192">As an example, let's look at the following diagram:</span></span> 

<span data-ttu-id="c5902-193"><center>
![Aktiviteten tröskelvärdet exempel][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="c5902-193"><center>
![Activity Threshold Example][Image3]
</center></span></span>

<span data-ttu-id="c5902-194">Precis som tröskelvärden för NLB är aktiviteten tröskelvärden definierade per-mått via definitionen för klustret:</span><span class="sxs-lookup"><span data-stu-id="c5902-194">Just like Balancing Thresholds, Activity Thresholds are defined per-metric via the cluster definition:</span></span>

<span data-ttu-id="c5902-195">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="c5902-195">ClusterManifest.xml</span></span>

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

<span data-ttu-id="c5902-196">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="c5902-196">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

<span data-ttu-id="c5902-197">Tröskelvärden för belastningsutjämning och aktivitet är både knutna till ett specifikt mått - belastningsutjämning aktiveras bara om både NLB tröskelvärde och aktivitet tröskelvärde har överskridits för samma mått.</span><span class="sxs-lookup"><span data-stu-id="c5902-197">Balancing and activity thresholds are both tied to a specific metric - balancing is triggered only if both the Balancing Threshold and Activity Threshold is exceeded for the same metric.</span></span>

## <a name="balancing-services-together"></a><span data-ttu-id="c5902-198">NLB tjänster tillsammans</span><span class="sxs-lookup"><span data-stu-id="c5902-198">Balancing services together</span></span>
<span data-ttu-id="c5902-199">Om klustret är imbalanced eller inte är ett hela beslut.</span><span class="sxs-lookup"><span data-stu-id="c5902-199">Whether the cluster is imbalanced or not is a cluster-wide decision.</span></span> <span data-ttu-id="c5902-200">Hur vi gå om hur du löser det flyttas enskild tjänst repliker och instanser runt.</span><span class="sxs-lookup"><span data-stu-id="c5902-200">However, the way we go about fixing it is moving individual service replicas and instances around.</span></span> <span data-ttu-id="c5902-201">Det låter väl logiskt, rätt?</span><span class="sxs-lookup"><span data-stu-id="c5902-201">This makes sense, right?</span></span> <span data-ttu-id="c5902-202">Om minnet är Staplad på en nod, kan flera repliker eller instanser bidra till den.</span><span class="sxs-lookup"><span data-stu-id="c5902-202">If memory is stacked up on one node, multiple replicas or instances could be contributing to it.</span></span> <span data-ttu-id="c5902-203">Åtgärda obalansen kan behöva flytta alla tillståndskänslig repliker och tillståndslösa instanser som använder imbalanced mått.</span><span class="sxs-lookup"><span data-stu-id="c5902-203">Fixing the imbalance could require moving any of the stateful replicas or stateless instances that use the imbalanced metric.</span></span>

<span data-ttu-id="c5902-204">Ibland om en tjänst som inte är själva imbalanced flyttas (Kom ihåg diskussion av lokala och globala viktas tidigare).</span><span class="sxs-lookup"><span data-stu-id="c5902-204">Occasionally though, a service that wasn’t itself imbalanced gets moved (remember the discussion of local and global weights earlier).</span></span> <span data-ttu-id="c5902-205">Varför skulle en tjänst flyttas när allt som tjänstens mått har balanserade?</span><span class="sxs-lookup"><span data-stu-id="c5902-205">Why would a service get moved when all that service’s metrics were balanced?</span></span> <span data-ttu-id="c5902-206">Låt oss se ett exempel:</span><span class="sxs-lookup"><span data-stu-id="c5902-206">Let’s see an example:</span></span>

- <span data-ttu-id="c5902-207">Anta att det finns fyra tjänster, Service1, plats2, tjänst3 och Service4.</span><span class="sxs-lookup"><span data-stu-id="c5902-207">Let's say there are four services, Service1, Service2, Service3, and Service4.</span></span> 
- <span data-ttu-id="c5902-208">Service1 rapporterar mått Metric1 och Metric2.</span><span class="sxs-lookup"><span data-stu-id="c5902-208">Service1 reports metrics Metric1 and Metric2.</span></span> 
- <span data-ttu-id="c5902-209">Plats2 rapporterar mått Metric2 och Metric3.</span><span class="sxs-lookup"><span data-stu-id="c5902-209">Service2 reports metrics Metric2 and Metric3.</span></span> 
- <span data-ttu-id="c5902-210">Tjänst3 rapporterar mått Metric3 och Metric4.</span><span class="sxs-lookup"><span data-stu-id="c5902-210">Service3 reports metrics Metric3 and Metric4.</span></span>
- <span data-ttu-id="c5902-211">Service4 rapporterar mått Metric99.</span><span class="sxs-lookup"><span data-stu-id="c5902-211">Service4 reports metric Metric99.</span></span> 

<span data-ttu-id="c5902-212">Du kan visserligen se där vi här: det finns en kedja!</span><span class="sxs-lookup"><span data-stu-id="c5902-212">Surely you can see where we’re going here: There's a chain!</span></span> <span data-ttu-id="c5902-213">Vi har inte fyra oberoende tjänster har vi tre tjänster som är relaterade och ett som är inaktiverat på sin egen.</span><span class="sxs-lookup"><span data-stu-id="c5902-213">We don’t really have four independent services, we have three services that are related and one that is off on its own.</span></span>

<span data-ttu-id="c5902-214"><center>
![NLB tjänster tillsammans][Image4]
</center></span><span class="sxs-lookup"><span data-stu-id="c5902-214"><center>
![Balancing Services Together][Image4]
</center></span></span>

<span data-ttu-id="c5902-215">På grund av kedjan är det möjligt att obalans i mått 1-4 kan orsaka repliker eller instanser som tillhör services 1-3 för att flytta.</span><span class="sxs-lookup"><span data-stu-id="c5902-215">Because of this chain, it's possible that an imbalance in metrics 1-4 can cause replicas or instances belonging to services 1-3 to move around.</span></span> <span data-ttu-id="c5902-216">Vi vet att en obalans i mått 1, 2 eller 3 kan orsaka förflyttningar i Service4.</span><span class="sxs-lookup"><span data-stu-id="c5902-216">We also know that an imbalance in Metrics 1, 2, or 3 can't cause movements in Service4.</span></span> <span data-ttu-id="c5902-217">Det är ingen sedan flytta replikerna eller instanser som tillhör Service4 runt kan absolut ingenting om du vill påverka balans mellan mått 1-3.</span><span class="sxs-lookup"><span data-stu-id="c5902-217">There would be no point since moving the replicas or instances belonging to Service4 around can do absolutely nothing to impact the balance of Metrics 1-3.</span></span>

<span data-ttu-id="c5902-218">Klustret Resource Manager siffror automatiskt reda på vilka tjänster som är relaterade.</span><span class="sxs-lookup"><span data-stu-id="c5902-218">The Cluster Resource Manager automatically figures out what services are related.</span></span> <span data-ttu-id="c5902-219">Lägga till, ta bort eller ändra måtten för tjänster som kan påverka deras relationer.</span><span class="sxs-lookup"><span data-stu-id="c5902-219">Adding, removing, or changing the metrics for services can impact their relationships.</span></span> <span data-ttu-id="c5902-220">Till exempel kan mellan två körningar av belastningsutjämning plats2 ha uppdaterats för att ta bort Metric2.</span><span class="sxs-lookup"><span data-stu-id="c5902-220">For example, between two runs of balancing Service2 may have been updated to remove Metric2.</span></span> <span data-ttu-id="c5902-221">Detta innebär att kedjan mellan Service1 och plats2.</span><span class="sxs-lookup"><span data-stu-id="c5902-221">This breaks the chain between Service1 and Service2.</span></span> <span data-ttu-id="c5902-222">Nu i stället för två grupper av relaterade tjänster finns tre:</span><span class="sxs-lookup"><span data-stu-id="c5902-222">Now instead of two groups of related services, there are three:</span></span>

<span data-ttu-id="c5902-223"><center>
![NLB tjänster tillsammans][Image5]
</center></span><span class="sxs-lookup"><span data-stu-id="c5902-223"><center>
![Balancing Services Together][Image5]
</center></span></span>

## <a name="next-steps"></a><span data-ttu-id="c5902-224">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5902-224">Next steps</span></span>
* <span data-ttu-id="c5902-225">Mått är hur Service Fabric-kluster Resource Manager hanterar förbrukning och kapacitet i klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-225">Metrics are how the Service Fabric Cluster Resource Manger manages consumption and capacity in the cluster.</span></span> <span data-ttu-id="c5902-226">Mer information om mått och hur du konfigurerar dem kolla [den här artikeln](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="c5902-226">To learn more about metrics and how to configure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>
* <span data-ttu-id="c5902-227">Förflyttningskostnad är ett sätt att signalering till klustret Resource Manager att vissa tjänster är dyrare att flytta än andra.</span><span class="sxs-lookup"><span data-stu-id="c5902-227">Movement Cost is one way of signaling to the Cluster Resource Manager that certain services are more expensive to move than others.</span></span> <span data-ttu-id="c5902-228">Mer information om förflyttningskostnad avser [i den här artikeln](service-fabric-cluster-resource-manager-movement-cost.md)</span><span class="sxs-lookup"><span data-stu-id="c5902-228">For more about movement cost, refer to [this article](service-fabric-cluster-resource-manager-movement-cost.md)</span></span>
* <span data-ttu-id="c5902-229">Klustret Resource Manager har flera begränsningar som du kan konfigurera långsammare kärnan i klustret.</span><span class="sxs-lookup"><span data-stu-id="c5902-229">The Cluster Resource Manager has several throttles that you can configure to slow down churn in the cluster.</span></span> <span data-ttu-id="c5902-230">De inte normalt krävs, men om du behöver dem kan du läsa om dem [här](service-fabric-cluster-resource-manager-advanced-throttling.md)</span><span class="sxs-lookup"><span data-stu-id="c5902-230">They're not normally necessary, but if you need them you can learn about them [here](service-fabric-cluster-resource-manager-advanced-throttling.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
