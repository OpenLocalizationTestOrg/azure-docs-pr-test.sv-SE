---
title: aaaBalance Azure Service Fabric-kluster | Microsoft Docs
description: "En introduktion toobalancing klustret med hello resurshanteraren för Service Fabric-klustret."
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
ms.openlocfilehash: 5f7ad2f5cf4cfb3751a860f5293b03d2d5266d99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="balancing-your-service-fabric-cluster"></a><span data-ttu-id="0d371-103">NLB service fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="0d371-103">Balancing your service fabric cluster</span></span>
<span data-ttu-id="0d371-104">hello resurshanteraren för Service Fabric-klustret har stöd för dynamisk ändringar, korsreagerande tooadditions eller borttagningar av noder eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="0d371-104">hello Service Fabric Cluster Resource Manager supports dynamic load changes, reacting tooadditions or removals of nodes or services.</span></span> <span data-ttu-id="0d371-105">Den korrigerar också automatiskt begränsningen överträdelser och balanserar proaktivt hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0d371-105">It also automatically corrects constraint violations, and proactively rebalances hello cluster.</span></span> <span data-ttu-id="0d371-106">Hur ofta dessa åtgärder vidtas för men vad utlöser dem?</span><span class="sxs-lookup"><span data-stu-id="0d371-106">But how often are these actions taken, and what triggers them?</span></span>

<span data-ttu-id="0d371-107">Det finns tre olika kategorier av arbete som hello klustret Resource Manager utför.</span><span class="sxs-lookup"><span data-stu-id="0d371-107">There are three different categories of work that hello Cluster Resource Manager performs.</span></span> <span data-ttu-id="0d371-108">De är:</span><span class="sxs-lookup"><span data-stu-id="0d371-108">They are:</span></span>

1. <span data-ttu-id="0d371-109">Placering – det här steget behandlar placera alla tillståndskänslig repliker eller tillståndslösa instanser som saknas.</span><span class="sxs-lookup"><span data-stu-id="0d371-109">Placement – this stage deals with placing any stateful replicas or stateless instances that are missing.</span></span> <span data-ttu-id="0d371-110">Placering innehåller både nya tjänster och hanterar tillståndskänslig repliker eller tillståndslösa instanser som har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="0d371-110">Placement includes both new services and handling stateful replicas or stateless instances that have failed.</span></span> <span data-ttu-id="0d371-111">Ta bort och släppa repliker eller instanser hanteras här.</span><span class="sxs-lookup"><span data-stu-id="0d371-111">Deleting and dropping replicas or instances are handled here.</span></span>
2. <span data-ttu-id="0d371-112">Begränsningen kontrollerar – det här steget kontrollerar och korrigerar brott mot hello olika placeringen (regler) inom hello system.</span><span class="sxs-lookup"><span data-stu-id="0d371-112">Constraint Checks – this stage checks for and corrects violations of hello different placement constraints (rules) within hello system.</span></span> <span data-ttu-id="0d371-113">Exempel på regler är till exempel att säkerställa att noderna inte är överbelastade och att en tjänst placeringen är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="0d371-113">Examples of rules are things like ensuring that nodes are not over capacity and that a service’s placement constraints are met.</span></span>
3. <span data-ttu-id="0d371-114">Belastningsutjämning – det här steget kontrollerar toosee om ombalansering behövs baserat på konfigurerad hello önskad nivå av balansera för olika mått.</span><span class="sxs-lookup"><span data-stu-id="0d371-114">Balancing – this stage checks toosee if rebalancing is necessary based on hello configured desired level of balance for different metrics.</span></span> <span data-ttu-id="0d371-115">I så fall försöker toofind en ordning i hello kluster som är mer belastningsutjämnade.</span><span class="sxs-lookup"><span data-stu-id="0d371-115">If so it attempts toofind an arrangement in hello cluster that is more balanced.</span></span>

## <a name="configuring-cluster-resource-manager-timers"></a><span data-ttu-id="0d371-116">Konfigurera Timers för hanteraren för filserverresurser</span><span class="sxs-lookup"><span data-stu-id="0d371-116">Configuring Cluster Resource Manager Timers</span></span>
<span data-ttu-id="0d371-117">hello första uppsättning kontroller runt belastningsutjämning är en uppsättning timers.</span><span class="sxs-lookup"><span data-stu-id="0d371-117">hello first set of controls around balancing are a set of timers.</span></span> <span data-ttu-id="0d371-118">Dessa timers styr hur ofta hello klustret Resource Manager undersöker hello klustret och tar korrigerande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0d371-118">These timers govern how often hello Cluster Resource Manager examines hello cluster and takes corrective actions.</span></span>

<span data-ttu-id="0d371-119">Var och en av dessa olika typer av korrigeringar hello klustret Resource Manager kan göra styrs av en annan timer som styr frekvensen.</span><span class="sxs-lookup"><span data-stu-id="0d371-119">Each of these different types of corrections hello Cluster Resource Manager can make is controlled by a different timer that governs its frequency.</span></span> <span data-ttu-id="0d371-120">När varje timer utlöses schemaläggs hello.</span><span class="sxs-lookup"><span data-stu-id="0d371-120">When each timer fires, hello task is scheduled.</span></span> <span data-ttu-id="0d371-121">Hej Resource Manager som standard:</span><span class="sxs-lookup"><span data-stu-id="0d371-121">By default hello Resource Manager:</span></span>

* <span data-ttu-id="0d371-122">genomsöker dess tillstånd och tillämpar uppdateringar (till exempel inspelningen som en nod nedåt) var 1/10 sekunder</span><span class="sxs-lookup"><span data-stu-id="0d371-122">scans its state and applies updates (like recording that a node is down) every 1/10th of a second</span></span>
* <span data-ttu-id="0d371-123">Anger hello placering Kontrollera flagga</span><span class="sxs-lookup"><span data-stu-id="0d371-123">sets hello placement check flag</span></span> 
* <span data-ttu-id="0d371-124">Anger att hello begränsningen kontrollera varje sekund</span><span class="sxs-lookup"><span data-stu-id="0d371-124">sets hello constraint check flag every second</span></span>
* <span data-ttu-id="0d371-125">Anger hello NLB flaggan var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="0d371-125">sets hello balancing flag every five seconds.</span></span>

<span data-ttu-id="0d371-126">Exempel på hello configuration styr dessa timers är nedan:</span><span class="sxs-lookup"><span data-stu-id="0d371-126">Examples of hello configuration governing these timers are below:</span></span>

<span data-ttu-id="0d371-127">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="0d371-127">ClusterManifest.xml:</span></span>

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

<span data-ttu-id="0d371-128">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="0d371-128">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

<span data-ttu-id="0d371-129">Idag utför hello klustret Resource Manager endast en av dessa åtgärder samtidigt, sekventiellt.</span><span class="sxs-lookup"><span data-stu-id="0d371-129">Today hello Cluster Resource Manager only performs one of these actions at a time, sequentially.</span></span> <span data-ttu-id="0d371-130">Det är därför vi finns toothese timers som ”minsta intervall” och hello åtgärder som hämta vidtas när hello timers går som ”inställningen flaggor”.</span><span class="sxs-lookup"><span data-stu-id="0d371-130">This is why we refer toothese timers as “minimum intervals” and hello actions that get taken when hello timers go off as "setting flags".</span></span> <span data-ttu-id="0d371-131">Hello klustret Resource Manager sköter väntande begäranden till exempel toocreate services innan NLB hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0d371-131">For example, hello Cluster Resource Manager takes care of pending requests toocreate services before balancing hello cluster.</span></span> <span data-ttu-id="0d371-132">Som du ser hello standard tidsintervall som angetts söker hello klustret Resource Manager efter något den behov toodo ofta.</span><span class="sxs-lookup"><span data-stu-id="0d371-132">As you can see by hello default time intervals specified, hello Cluster Resource Manager scans for anything it needs toodo frequently.</span></span> <span data-ttu-id="0d371-133">Detta innebär normalt att hello uppsättning ändringar som gjorts under varje steg är liten.</span><span class="sxs-lookup"><span data-stu-id="0d371-133">Normally this means that hello set of changes made during each step is small.</span></span> <span data-ttu-id="0d371-134">Att göra små ändringar ofta tillåter hello klustret Resource Manager toobe responsiv när saker i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="0d371-134">Making small changes frequently allows hello Cluster Resource Manager toobe responsive when things happen in hello cluster.</span></span> <span data-ttu-id="0d371-135">hello standard timers ange vissa batchbearbetning eftersom många hello samma typer av händelser tenderar toooccur samtidigt.</span><span class="sxs-lookup"><span data-stu-id="0d371-135">hello default timers provide some batching since many of hello same types of events tend toooccur simultaneously.</span></span> 

<span data-ttu-id="0d371-136">Till exempel när noder misslyckas de kan göra så att hela feldomäner i taget.</span><span class="sxs-lookup"><span data-stu-id="0d371-136">For example, when nodes fail they can do so entire fault domains at a time.</span></span> <span data-ttu-id="0d371-137">Alla dessa fel avbildas under hello nästa tillstånd uppdatera efter hello *PLBRefreshGap*.</span><span class="sxs-lookup"><span data-stu-id="0d371-137">All these failures are captured during hello next state update after hello *PLBRefreshGap*.</span></span> <span data-ttu-id="0d371-138">hello korrigeringar fastställs under hello efter placering, kontroll-begränsning, och NLB körs.</span><span class="sxs-lookup"><span data-stu-id="0d371-138">hello corrections are determined during hello following placement, constraint check, and balancing runs.</span></span> <span data-ttu-id="0d371-139">Som standard hello klustret Resource Manager inte skanna via timmar för ändringar i hello kluster och försök tooaddress alla ändringar på en gång.</span><span class="sxs-lookup"><span data-stu-id="0d371-139">By default hello Cluster Resource Manager is not scanning through hours of changes in hello cluster and trying tooaddress all changes at once.</span></span> <span data-ttu-id="0d371-140">Det skulle kunna leda toobursts av omsättning.</span><span class="sxs-lookup"><span data-stu-id="0d371-140">Doing so would lead toobursts of churn.</span></span>

<span data-ttu-id="0d371-141">hello klustret Resource Manager måste också toodetermine vissa ytterligare information om hello kluster imbalanced.</span><span class="sxs-lookup"><span data-stu-id="0d371-141">hello Cluster Resource Manager also needs some additional information toodetermine if hello cluster imbalanced.</span></span> <span data-ttu-id="0d371-142">Som vi har två delar i konfigurationen: *BalancingThresholds* och *ActivityThresholds*.</span><span class="sxs-lookup"><span data-stu-id="0d371-142">For that we have two other pieces of configuration: *BalancingThresholds* and *ActivityThresholds*.</span></span>

## <a name="balancing-thresholds"></a><span data-ttu-id="0d371-143">NLB tröskelvärden</span><span class="sxs-lookup"><span data-stu-id="0d371-143">Balancing thresholds</span></span>
<span data-ttu-id="0d371-144">Ett tröskelvärde för NLB är hello huvudsakliga kontroll för att utlösa ombalansering.</span><span class="sxs-lookup"><span data-stu-id="0d371-144">A Balancing Threshold is hello main control for triggering rebalancing.</span></span> <span data-ttu-id="0d371-145">hello NLB tröskelvärdet för ett mått är ett _förhållandet_.</span><span class="sxs-lookup"><span data-stu-id="0d371-145">hello Balancing Threshold for a metric is a _ratio_.</span></span> <span data-ttu-id="0d371-146">Om hello belastning för ett mått på hello mest inläst nod dividerat med hello mängden belastningen på hello minst inlästa nod överskrider den måtten *BalancingThreshold*, och sedan hello klustret är imbalanced.</span><span class="sxs-lookup"><span data-stu-id="0d371-146">If hello load for a metric on hello most loaded node divided by hello amount of load on hello least loaded node exceeds that metric's *BalancingThreshold*, then hello cluster is imbalanced.</span></span> <span data-ttu-id="0d371-147">Därför är utlösta hello nästa gång hello klustret Resource Manager kontrollerar.</span><span class="sxs-lookup"><span data-stu-id="0d371-147">As a result balancing is triggered hello next time hello Cluster Resource Manager checks.</span></span> <span data-ttu-id="0d371-148">Hej *MinLoadBalancingInterval* timer definierar hur ofta hello klustret Resource Manager ska kontrollera om ombalansering krävs.</span><span class="sxs-lookup"><span data-stu-id="0d371-148">hello *MinLoadBalancingInterval* timer defines how often hello Cluster Resource Manager should check if rebalancing is necessary.</span></span> <span data-ttu-id="0d371-149">Kontrollera innebär inte att något händer.</span><span class="sxs-lookup"><span data-stu-id="0d371-149">Checking doesn't mean that anything happens.</span></span> 

<span data-ttu-id="0d371-150">Tröskelvärden för belastningsutjämning definieras på grundval av per mått som en del av hello klustret definition.</span><span class="sxs-lookup"><span data-stu-id="0d371-150">Balancing Thresholds are defined on a per-metric basis as a part of hello cluster definition.</span></span> <span data-ttu-id="0d371-151">Mer information om mått kolla [i den här artikeln](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="0d371-151">For more information on metrics, check out [this article](service-fabric-cluster-resource-manager-metrics.md).</span></span>

<span data-ttu-id="0d371-152">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="0d371-152">ClusterManifest.xml</span></span>

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

<span data-ttu-id="0d371-153">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="0d371-153">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

<span data-ttu-id="0d371-154"><center>
![Belastningsutjämning tröskelvärde-exempel][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="0d371-154"><center>
![Balancing Threshold Example][Image1]
</center></span></span>

<span data-ttu-id="0d371-155">I det här exemplet förbrukar en enhet av vissa mått för varje tjänst.</span><span class="sxs-lookup"><span data-stu-id="0d371-155">In this example, each service is consuming one unit of some metric.</span></span> <span data-ttu-id="0d371-156">I hello översta exempelvis hello belastningen på en nod är fem och hello minsta är två.</span><span class="sxs-lookup"><span data-stu-id="0d371-156">In hello top example, hello maximum load on a node is five and hello minimum is two.</span></span> <span data-ttu-id="0d371-157">Anta att hello NLB tröskelvärdet för det här måttet är tre.</span><span class="sxs-lookup"><span data-stu-id="0d371-157">Let’s say that hello balancing threshold for this metric is three.</span></span> <span data-ttu-id="0d371-158">Eftersom hello förhållandet i hello kluster är 5/2 = 2,5 och som är mindre än hello angivet tröskelvärde i tre, hello-kluster är belastningsutjämnat.</span><span class="sxs-lookup"><span data-stu-id="0d371-158">Since hello ratio in hello cluster is 5/2 = 2.5 and that is less than hello specified balancing threshold of three, hello cluster is balanced.</span></span> <span data-ttu-id="0d371-159">Ingen belastningsutjämning utlöses när hello klustret Resource Manager kontrollerar.</span><span class="sxs-lookup"><span data-stu-id="0d371-159">No balancing is triggered when hello Cluster Resource Manager checks.</span></span>

<span data-ttu-id="0d371-160">I hello ned exempel är hello belastningen på en nod 10, medan hello minst två, vilket resulterar i ett förhållande på fem.</span><span class="sxs-lookup"><span data-stu-id="0d371-160">In hello bottom example, hello maximum load on a node is 10, while hello minimum is two, resulting in a ratio of five.</span></span> <span data-ttu-id="0d371-161">Fem är större än hello avsedda belastningsutjämning tröskelvärdet på tre för den måtten.</span><span class="sxs-lookup"><span data-stu-id="0d371-161">Five is greater than hello designated balancing threshold of three for that metric.</span></span> <span data-ttu-id="0d371-162">Detta innebär att en rebalancing kör schemalagda nästa gång hello NLB timer utlöses.</span><span class="sxs-lookup"><span data-stu-id="0d371-162">As a result, a rebalancing run will be scheduled next time hello balancing timer fires.</span></span> <span data-ttu-id="0d371-163">I den här situationen är vissa belastningen vanligtvis distribuerade tooNode3.</span><span class="sxs-lookup"><span data-stu-id="0d371-163">In a situation like this some load is usually distributed tooNode3.</span></span> <span data-ttu-id="0d371-164">Eftersom hello resurshanteraren för Service Fabric-klustret inte använder en girig metod, kan vissa belastningen också vara distribuerade tooNode2.</span><span class="sxs-lookup"><span data-stu-id="0d371-164">Because hello Service Fabric Cluster Resource Manager doesn't use a greedy approach, some load could also be distributed tooNode2.</span></span> 

<span data-ttu-id="0d371-165"><center>
![Belastningsutjämning tröskelvärdet exempel åtgärder][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="0d371-165"><center>
![Balancing Threshold Example Actions][Image2]
</center></span></span>

> [!NOTE]
> <span data-ttu-id="0d371-166">”NLB” hanterar två olika strategier för att hantera belastningen i klustret.</span><span class="sxs-lookup"><span data-stu-id="0d371-166">"Balancing" handles two different strategies for managing load in your cluster.</span></span> <span data-ttu-id="0d371-167">hello standard strategi som hello hanteraren för filserverresurser använder är toodistribute belastningen över hello noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="0d371-167">hello default strategy that hello Cluster Resource Manager uses is toodistribute load across hello nodes in hello cluster.</span></span> <span data-ttu-id="0d371-168">hello andra strategin är [defragmentering](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="0d371-168">hello other strategy is [defragmentation](service-fabric-cluster-resource-manager-defragmentation-metrics.md).</span></span> <span data-ttu-id="0d371-169">Defragmentering utförs under hello kör samma belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="0d371-169">Defragmentation is performed during hello same balancing run.</span></span> <span data-ttu-id="0d371-170">hello belastningsutjämning och defragmentering strategier kan användas för olika mått i hello samma kluster.</span><span class="sxs-lookup"><span data-stu-id="0d371-170">hello balancing and defragmentation strategies can be used for different metrics within hello same cluster.</span></span> <span data-ttu-id="0d371-171">En tjänst kan ha belastningsutjämning och defragmentering mått.</span><span class="sxs-lookup"><span data-stu-id="0d371-171">A service can have both balancing and defragmentation metrics.</span></span> <span data-ttu-id="0d371-172">För defragmentering mätvärden hello förhållandet mellan hello läses in i hello klustret utlösare ombalansering när det är _nedan_ hello NLB tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="0d371-172">For defragmentation metrics, hello ratio of hello loads in hello cluster triggers rebalancing when it is _below_ hello balancing threshold.</span></span> 
>

<span data-ttu-id="0d371-173">Hämta nedan hello NLB tröskelvärdet är inte en explicit målet.</span><span class="sxs-lookup"><span data-stu-id="0d371-173">Getting below hello balancing threshold is not an explicit goal.</span></span> <span data-ttu-id="0d371-174">Tröskelvärden för belastningsutjämning är bara en *utlösaren*.</span><span class="sxs-lookup"><span data-stu-id="0d371-174">Balancing Thresholds are just a *trigger*.</span></span> <span data-ttu-id="0d371-175">När NLB körs avgör hello klustret Resource Manager vilka förbättringar som det kan göra eventuella.</span><span class="sxs-lookup"><span data-stu-id="0d371-175">When balancing runs, hello Cluster Resource Manager determines what improvements it can make, if any.</span></span> <span data-ttu-id="0d371-176">Bara för att en belastningsutjämning sökning har inletts innebär inte något flyttas.</span><span class="sxs-lookup"><span data-stu-id="0d371-176">Just because a balancing search is kicked off doesn't mean anything moves.</span></span> <span data-ttu-id="0d371-177">Ibland är hello klustret imbalanced men för begränsad toocorrect.</span><span class="sxs-lookup"><span data-stu-id="0d371-177">Sometimes hello cluster is imbalanced but too constrained toocorrect.</span></span> <span data-ttu-id="0d371-178">Alternativt hello förbättringar kräver förflyttningar för [kostsamma](service-fabric-cluster-resource-manager-movement-cost.md)).</span><span class="sxs-lookup"><span data-stu-id="0d371-178">Alternatively, hello improvements require movements that are too [costly](service-fabric-cluster-resource-manager-movement-cost.md)).</span></span>

## <a name="activity-thresholds"></a><span data-ttu-id="0d371-179">Tröskelvärden för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="0d371-179">Activity thresholds</span></span>
<span data-ttu-id="0d371-180">Ibland men noder är relativt imbalanced, hello *totala* belastningen i hello kluster är låg.</span><span class="sxs-lookup"><span data-stu-id="0d371-180">Sometimes, although nodes are relatively imbalanced, hello *total* amount of load in hello cluster is low.</span></span> <span data-ttu-id="0d371-181">hello bristande belastning kan vara tillfälliga dip eller eftersom hello kluster är ny och bara hämtning startade.</span><span class="sxs-lookup"><span data-stu-id="0d371-181">hello lack of load could be a transient dip, or because hello cluster is new and just getting bootstrapped.</span></span> <span data-ttu-id="0d371-182">I båda fallen kan du inte vill toospend tid NLB hello klustret eftersom det finns lite toobe erfarenheter.</span><span class="sxs-lookup"><span data-stu-id="0d371-182">In either case, you may not want toospend time balancing hello cluster because there’s little toobe gained.</span></span> <span data-ttu-id="0d371-183">Om hello klustret genomgått belastningsutjämning, skulle du ägnar åt nätverket och beräkna resurser toomove runt saker och ting utan att göra några stora *absolut* skillnaden.</span><span class="sxs-lookup"><span data-stu-id="0d371-183">If hello cluster underwent balancing, you’d spend network and compute resources toomove things around without making any large *absolute* difference.</span></span> <span data-ttu-id="0d371-184">tooavoid onödiga flyttar det finns en annan kontroll kallas tröskelvärden för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0d371-184">tooavoid unnecessary moves, there’s another control known as Activity Thresholds.</span></span> <span data-ttu-id="0d371-185">Aktiviteten tröskelvärden kan toospecify vissa absolut undre gränsvärde för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0d371-185">Activity Thresholds allows you toospecify some absolute lower bound for activity.</span></span> <span data-ttu-id="0d371-186">Om ingen nod är över tröskeln, utlösas belastningsutjämning inte även om hello NLB tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="0d371-186">If no node is over this threshold, balancing isn't triggered even if hello Balancing Threshold is met.</span></span>

<span data-ttu-id="0d371-187">Anta att vi behåller våra tröskelvärdet för belastningsutjämning av tre för det här måttet.</span><span class="sxs-lookup"><span data-stu-id="0d371-187">Let’s say that we retain our Balancing Threshold of three for this metric.</span></span> <span data-ttu-id="0d371-188">Anta också att vi har ett tröskelvärde för aktiviteten på 1536.</span><span class="sxs-lookup"><span data-stu-id="0d371-188">Let's also say we have an Activity Threshold of 1536.</span></span> <span data-ttu-id="0d371-189">I hello första fallet medan hello klustret är imbalanced per hello NLB tröskelvärdet det är ingen nod uppfyller aktivitet gränsen, så ingenting händer.</span><span class="sxs-lookup"><span data-stu-id="0d371-189">In hello first case, while hello cluster is imbalanced per hello Balancing Threshold there's no node meets that Activity Threshold, so nothing happens.</span></span> <span data-ttu-id="0d371-190">I hello nedre exempelvis är Nod1 över hello aktivitet tröskelvärdet.</span><span class="sxs-lookup"><span data-stu-id="0d371-190">In hello bottom example, Node1 is over hello Activity Threshold.</span></span> <span data-ttu-id="0d371-191">Eftersom både hello tröskelvärdet för belastningsutjämning och hello aktivitet tröskelvärdet för hello mått överskrids, har belastningsutjämning schemalagts.</span><span class="sxs-lookup"><span data-stu-id="0d371-191">Since both hello Balancing Threshold and hello Activity Threshold for hello metric are exceeded, balancing is scheduled.</span></span> <span data-ttu-id="0d371-192">Exempelvis ska vi titta på hello följande diagram:</span><span class="sxs-lookup"><span data-stu-id="0d371-192">As an example, let's look at hello following diagram:</span></span> 

<span data-ttu-id="0d371-193"><center>
![Aktiviteten tröskelvärdet exempel][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="0d371-193"><center>
![Activity Threshold Example][Image3]
</center></span></span>

<span data-ttu-id="0d371-194">Precis som tröskelvärden för NLB är aktiviteten tröskelvärden definierade per-mått via hello klustret definition:</span><span class="sxs-lookup"><span data-stu-id="0d371-194">Just like Balancing Thresholds, Activity Thresholds are defined per-metric via hello cluster definition:</span></span>

<span data-ttu-id="0d371-195">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="0d371-195">ClusterManifest.xml</span></span>

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

<span data-ttu-id="0d371-196">värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:</span><span class="sxs-lookup"><span data-stu-id="0d371-196">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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

<span data-ttu-id="0d371-197">Tröskelvärden för belastningsutjämning och aktivitet är bundet tooa specifikt mått - belastningsutjämning utlöses endast om båda hello tröskelvärdet för belastningsutjämning och aktivitet tröskelvärde har överskridits för hello samma mått.</span><span class="sxs-lookup"><span data-stu-id="0d371-197">Balancing and activity thresholds are both tied tooa specific metric - balancing is triggered only if both hello Balancing Threshold and Activity Threshold is exceeded for hello same metric.</span></span>

## <a name="balancing-services-together"></a><span data-ttu-id="0d371-198">NLB tjänster tillsammans</span><span class="sxs-lookup"><span data-stu-id="0d371-198">Balancing services together</span></span>
<span data-ttu-id="0d371-199">Om hello klustret är imbalanced eller inte är ett hela beslut.</span><span class="sxs-lookup"><span data-stu-id="0d371-199">Whether hello cluster is imbalanced or not is a cluster-wide decision.</span></span> <span data-ttu-id="0d371-200">Hello sätt vi gå om hur du löser det flyttas enskild tjänst repliker och instanser runt.</span><span class="sxs-lookup"><span data-stu-id="0d371-200">However, hello way we go about fixing it is moving individual service replicas and instances around.</span></span> <span data-ttu-id="0d371-201">Det låter väl logiskt, rätt?</span><span class="sxs-lookup"><span data-stu-id="0d371-201">This makes sense, right?</span></span> <span data-ttu-id="0d371-202">Om minnet är Staplad på en nod, kan flera repliker eller instanser bidra tooit.</span><span class="sxs-lookup"><span data-stu-id="0d371-202">If memory is stacked up on one node, multiple replicas or instances could be contributing tooit.</span></span> <span data-ttu-id="0d371-203">Åtgärdar hello obalans kan det krävas flytta hello tillståndskänslig repliker eller tillståndslösa instanser som använder hello imbalanced mått.</span><span class="sxs-lookup"><span data-stu-id="0d371-203">Fixing hello imbalance could require moving any of hello stateful replicas or stateless instances that use hello imbalanced metric.</span></span>

<span data-ttu-id="0d371-204">Ibland om en tjänst som inte är själva imbalanced flyttas (Kom ihåg hello diskussion av lokala och globala viktas tidigare).</span><span class="sxs-lookup"><span data-stu-id="0d371-204">Occasionally though, a service that wasn’t itself imbalanced gets moved (remember hello discussion of local and global weights earlier).</span></span> <span data-ttu-id="0d371-205">Varför skulle en tjänst flyttas när allt som tjänstens mått har balanserade?</span><span class="sxs-lookup"><span data-stu-id="0d371-205">Why would a service get moved when all that service’s metrics were balanced?</span></span> <span data-ttu-id="0d371-206">Låt oss se ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0d371-206">Let’s see an example:</span></span>

- <span data-ttu-id="0d371-207">Anta att det finns fyra tjänster, Service1, plats2, tjänst3 och Service4.</span><span class="sxs-lookup"><span data-stu-id="0d371-207">Let's say there are four services, Service1, Service2, Service3, and Service4.</span></span> 
- <span data-ttu-id="0d371-208">Service1 rapporterar mått Metric1 och Metric2.</span><span class="sxs-lookup"><span data-stu-id="0d371-208">Service1 reports metrics Metric1 and Metric2.</span></span> 
- <span data-ttu-id="0d371-209">Plats2 rapporterar mått Metric2 och Metric3.</span><span class="sxs-lookup"><span data-stu-id="0d371-209">Service2 reports metrics Metric2 and Metric3.</span></span> 
- <span data-ttu-id="0d371-210">Tjänst3 rapporterar mått Metric3 och Metric4.</span><span class="sxs-lookup"><span data-stu-id="0d371-210">Service3 reports metrics Metric3 and Metric4.</span></span>
- <span data-ttu-id="0d371-211">Service4 rapporterar mått Metric99.</span><span class="sxs-lookup"><span data-stu-id="0d371-211">Service4 reports metric Metric99.</span></span> 

<span data-ttu-id="0d371-212">Du kan visserligen se där vi här: det finns en kedja!</span><span class="sxs-lookup"><span data-stu-id="0d371-212">Surely you can see where we’re going here: There's a chain!</span></span> <span data-ttu-id="0d371-213">Vi har inte fyra oberoende tjänster har vi tre tjänster som är relaterade och ett som är inaktiverat på sin egen.</span><span class="sxs-lookup"><span data-stu-id="0d371-213">We don’t really have four independent services, we have three services that are related and one that is off on its own.</span></span>

<span data-ttu-id="0d371-214"><center>
![NLB tjänster tillsammans][Image4]
</center></span><span class="sxs-lookup"><span data-stu-id="0d371-214"><center>
![Balancing Services Together][Image4]
</center></span></span>

<span data-ttu-id="0d371-215">På grund av kedjan är det möjligt att obalans i mått 1-4 kan orsaka repliker eller instanser som tillhör tooservices 1-3 toomove runt.</span><span class="sxs-lookup"><span data-stu-id="0d371-215">Because of this chain, it's possible that an imbalance in metrics 1-4 can cause replicas or instances belonging tooservices 1-3 toomove around.</span></span> <span data-ttu-id="0d371-216">Vi vet att en obalans i mått 1, 2 eller 3 kan orsaka förflyttningar i Service4.</span><span class="sxs-lookup"><span data-stu-id="0d371-216">We also know that an imbalance in Metrics 1, 2, or 3 can't cause movements in Service4.</span></span> <span data-ttu-id="0d371-217">Det skulle inte finns några sedan flytta hello repliker eller instanser som tillhör tooService4 runt kan gör absolut ingenting tooimpact hello balans mellan mått 1-3.</span><span class="sxs-lookup"><span data-stu-id="0d371-217">There would be no point since moving hello replicas or instances belonging tooService4 around can do absolutely nothing tooimpact hello balance of Metrics 1-3.</span></span>

<span data-ttu-id="0d371-218">hello klustret Resource Manager siffror automatiskt reda på vilka tjänster som är relaterade.</span><span class="sxs-lookup"><span data-stu-id="0d371-218">hello Cluster Resource Manager automatically figures out what services are related.</span></span> <span data-ttu-id="0d371-219">Lägga till, ta bort eller ändra hello mått för tjänster kan påverka deras relationer.</span><span class="sxs-lookup"><span data-stu-id="0d371-219">Adding, removing, or changing hello metrics for services can impact their relationships.</span></span> <span data-ttu-id="0d371-220">Till exempel kan mellan två körningar av belastningsutjämning plats2 ha varit uppdaterade tooremove Metric2.</span><span class="sxs-lookup"><span data-stu-id="0d371-220">For example, between two runs of balancing Service2 may have been updated tooremove Metric2.</span></span> <span data-ttu-id="0d371-221">Detta innebär att hello kedjan mellan Service1 och plats2.</span><span class="sxs-lookup"><span data-stu-id="0d371-221">This breaks hello chain between Service1 and Service2.</span></span> <span data-ttu-id="0d371-222">Nu i stället för två grupper av relaterade tjänster finns tre:</span><span class="sxs-lookup"><span data-stu-id="0d371-222">Now instead of two groups of related services, there are three:</span></span>

<span data-ttu-id="0d371-223"><center>
![NLB tjänster tillsammans][Image5]
</center></span><span class="sxs-lookup"><span data-stu-id="0d371-223"><center>
![Balancing Services Together][Image5]
</center></span></span>

## <a name="next-steps"></a><span data-ttu-id="0d371-224">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d371-224">Next steps</span></span>
* <span data-ttu-id="0d371-225">Mått är hur hello Service Fabric-kluster Resource Manager hanterar förbrukning och kapacitet i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0d371-225">Metrics are how hello Service Fabric Cluster Resource Manger manages consumption and capacity in hello cluster.</span></span> <span data-ttu-id="0d371-226">Mer om mått toolearn och hur tooconfigure dem, kolla [den här artikeln](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="0d371-226">toolearn more about metrics and how tooconfigure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>
* <span data-ttu-id="0d371-227">Förflyttningskostnad är ett sätt att signalering toohello klustret Resource Manager att vissa tjänster är dyrare toomove än andra.</span><span class="sxs-lookup"><span data-stu-id="0d371-227">Movement Cost is one way of signaling toohello Cluster Resource Manager that certain services are more expensive toomove than others.</span></span> <span data-ttu-id="0d371-228">Mer information om förflyttningskostnad finns för[i den här artikeln](service-fabric-cluster-resource-manager-movement-cost.md)</span><span class="sxs-lookup"><span data-stu-id="0d371-228">For more about movement cost, refer too[this article](service-fabric-cluster-resource-manager-movement-cost.md)</span></span>
* <span data-ttu-id="0d371-229">hello klustret Resource Manager har flera begränsningar som du kan konfigurera tooslow ned omsättningen i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="0d371-229">hello Cluster Resource Manager has several throttles that you can configure tooslow down churn in hello cluster.</span></span> <span data-ttu-id="0d371-230">De inte normalt krävs, men om du behöver dem kan du läsa om dem [här](service-fabric-cluster-resource-manager-advanced-throttling.md)</span><span class="sxs-lookup"><span data-stu-id="0d371-230">They're not normally necessary, but if you need them you can learn about them [here](service-fabric-cluster-resource-manager-advanced-throttling.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
