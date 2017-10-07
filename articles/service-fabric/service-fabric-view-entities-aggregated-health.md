---
title: "aaaHow tooview Azure Service Fabric entiteternas samman hälsa | Microsoft Docs"
description: "Beskriver hur tooquery, visa och utvärdera Azure Service Fabric entiteter aggregerade hälsa genom hälsoförfrågningar och allmänna frågor."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: add810551cac26d2b4ff81b57d94ddd780c2cc2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-service-fabric-health-reports"></a><span data-ttu-id="b73be-103">Visa Service Fabric-hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="b73be-103">View Service Fabric health reports</span></span>
<span data-ttu-id="b73be-104">Azure Service Fabric introducerar en [hälsomodell](service-fabric-health-introduction.md) med hälsa entiteter där systemkomponenter och watchdogs kan rapporten lokala villkor som de övervakar.</span><span class="sxs-lookup"><span data-stu-id="b73be-104">Azure Service Fabric introduces a [health model](service-fabric-health-introduction.md) with health entities on which system components and watchdogs can report local conditions that they are monitoring.</span></span> <span data-ttu-id="b73be-105">Hej [hälsoarkivet](service-fabric-health-introduction.md#health-store) aggregerar alla hälsa data toodetermine om enheter är felfria.</span><span class="sxs-lookup"><span data-stu-id="b73be-105">hello [health store](service-fabric-health-introduction.md#health-store) aggregates all health data toodetermine whether entities are healthy.</span></span>

<span data-ttu-id="b73be-106">hello klustret fylls automatiskt med hälsorapporter som skickas av hello systemkomponenter.</span><span class="sxs-lookup"><span data-stu-id="b73be-106">hello cluster is automatically populated with health reports sent by hello system components.</span></span> <span data-ttu-id="b73be-107">Läs mer på [Använd systemhälsa rapporterar tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).</span><span class="sxs-lookup"><span data-stu-id="b73be-107">Read more at [Use system health reports tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).</span></span>

<span data-ttu-id="b73be-108">Service Fabric innehåller flera olika sätt tooget hello samman hälsotillståndet för hello enheter:</span><span class="sxs-lookup"><span data-stu-id="b73be-108">Service Fabric provides multiple ways tooget hello aggregated health of hello entities:</span></span>

* <span data-ttu-id="b73be-109">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) eller andra visualiseringsverktyg för</span><span class="sxs-lookup"><span data-stu-id="b73be-109">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) or other visualization tools</span></span>
* <span data-ttu-id="b73be-110">Hälsoförfrågningar (via PowerShell API eller REST)</span><span class="sxs-lookup"><span data-stu-id="b73be-110">Health queries (through PowerShell, API, or REST)</span></span>
* <span data-ttu-id="b73be-111">Allmänna frågor som returnerar en lista över enheter som har hälsa som en av hello-egenskaper (via PowerShell API eller REST)</span><span class="sxs-lookup"><span data-stu-id="b73be-111">General queries that return a list of entities that have health as one of hello properties (through PowerShell, API, or REST)</span></span>

<span data-ttu-id="b73be-112">toodemonstrate dessa alternativ, vi använder en lokal kluster med fem noder och hello [fabric: / WordCount programmet](http://aka.ms/servicefabric-wordcountapp).</span><span class="sxs-lookup"><span data-stu-id="b73be-112">toodemonstrate these options, let's use a local cluster with five nodes and hello [fabric:/WordCount application](http://aka.ms/servicefabric-wordcountapp).</span></span> <span data-ttu-id="b73be-113">Hej **fabric: / WordCount** program innehåller två standardtjänster är en tillståndskänslig tjänst av typen `WordCountServiceType`, och en tillståndslös tjänst av typen `WordCountWebServiceType`.</span><span class="sxs-lookup"><span data-stu-id="b73be-113">hello **fabric:/WordCount** application contains two default services, a stateful service of type `WordCountServiceType`, and a stateless service of type `WordCountWebServiceType`.</span></span> <span data-ttu-id="b73be-114">Du har ändrat hello `ApplicationManifest.xml` toorequire sju mål repliker för hello tillståndskänslig service och en partition.</span><span class="sxs-lookup"><span data-stu-id="b73be-114">I changed hello `ApplicationManifest.xml` toorequire seven target replicas for hello stateful service and one partition.</span></span> <span data-ttu-id="b73be-115">Eftersom det finns bara fem noder i klustret hello, rapport hello systemkomponenter en varning om hello service partitionen eftersom den är under hello målantalet.</span><span class="sxs-lookup"><span data-stu-id="b73be-115">Because there are only five nodes in hello cluster, hello system components report a warning on hello service partition because it is below hello target count.</span></span>

```xml
<Service Name="WordCountService">
<<<<<<< HEAD
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="3">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
=======
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
>>>>>>> 5e84dbdd8e45a5d6b36f435a550b7433b873bf11
</Service>
```

## <a name="health-in-service-fabric-explorer"></a><span data-ttu-id="b73be-116">Hälsa i Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="b73be-116">Health in Service Fabric Explorer</span></span>
<span data-ttu-id="b73be-117">Service Fabric Explorer ger en visuell översikt över hello klustret.</span><span class="sxs-lookup"><span data-stu-id="b73be-117">Service Fabric Explorer provides a visual view of hello cluster.</span></span> <span data-ttu-id="b73be-118">Du kan se som i hello bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="b73be-118">In hello image below, you can see that:</span></span>

* <span data-ttu-id="b73be-119">Hej programmet **fabric: / WordCount** är röd (fel) eftersom den har en felhändelse som rapporterats av **MyWatchdog** för hello egenskapen **tillgänglighet**.</span><span class="sxs-lookup"><span data-stu-id="b73be-119">hello application **fabric:/WordCount** is red (in error) because it has an error event reported by **MyWatchdog** for hello property **Availability**.</span></span>
* <span data-ttu-id="b73be-120">En av dess tjänster, **fabric: / WordCount/WordCountService** är gul (i varning).</span><span class="sxs-lookup"><span data-stu-id="b73be-120">One of its services, **fabric:/WordCount/WordCountService** is yellow (in warning).</span></span> <span data-ttu-id="b73be-121">hello-tjänsten har konfigurerats med sju repliker och hello klustret har fem noder så att två repicas inte kan placeras.</span><span class="sxs-lookup"><span data-stu-id="b73be-121">hello service is configured with seven replicas and hello cluster has five nodes, so two repicas can't be placed.</span></span> <span data-ttu-id="b73be-122">Även om det inte visas här, hello-tjänsten är gult på grund av en rapport från `System.FM` säger som `Partition is below target replica or instance count`.</span><span class="sxs-lookup"><span data-stu-id="b73be-122">Although it's not shown here, hello service partition is yellow because of a system report from `System.FM` saying that `Partition is below target replica or instance count`.</span></span> <span data-ttu-id="b73be-123">hello gul partition utlösare hello gul service.</span><span class="sxs-lookup"><span data-stu-id="b73be-123">hello yellow partition triggers hello yellow service.</span></span>
* <span data-ttu-id="b73be-124">hello kluster är red på grund av programmet hello röd.</span><span class="sxs-lookup"><span data-stu-id="b73be-124">hello cluster is red because of hello red application.</span></span>

<span data-ttu-id="b73be-125">hello utvärdering använder standardprinciper från hello klustermanifestet och programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="b73be-125">hello evaluation uses default policies from hello cluster manifest and application manifest.</span></span> <span data-ttu-id="b73be-126">De strikta principer och tolererar inte att ett fel.</span><span class="sxs-lookup"><span data-stu-id="b73be-126">They are strict policies and do not tolerate any failure.</span></span>

<span data-ttu-id="b73be-127">Vy över hello kluster med Service Fabric Explorer:</span><span class="sxs-lookup"><span data-stu-id="b73be-127">View of hello cluster with Service Fabric Explorer:</span></span>

![Vy över hello kluster med Service Fabric Explorer.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> <span data-ttu-id="b73be-129">Läs mer om [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="b73be-129">Read more about [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

## <a name="health-queries"></a><span data-ttu-id="b73be-130">Hälsoförfrågningar</span><span class="sxs-lookup"><span data-stu-id="b73be-130">Health queries</span></span>
<span data-ttu-id="b73be-131">Service Fabric exponerar hälsoförfrågningar för varje hello stöds [entitetstyper](service-fabric-health-introduction.md#health-entities-and-hierarchy).</span><span class="sxs-lookup"><span data-stu-id="b73be-131">Service Fabric exposes health queries for each of hello supported [entity types](service-fabric-health-introduction.md#health-entities-and-hierarchy).</span></span> <span data-ttu-id="b73be-132">De kan nås via hello API, med metoder på [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell-cmdletar och REST.</span><span class="sxs-lookup"><span data-stu-id="b73be-132">They can be accessed through hello API, using methods on [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell cmdlets, and REST.</span></span> <span data-ttu-id="b73be-133">De här frågorna returnera hela hälsotillståndet information om hello entitet: hello samman hälsotillstånd, entiteten hälsa händelser, underordnade hälsotillstånd (i förekommande fall), ohälsosamt utvärderingar (när hello entiteten inte är felfri) och underordnade hälsostatistik (när tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="b73be-133">These queries return complete health information about hello entity: hello aggregated health state, entity health events, child health states (when applicable), unhealthy evaluations (when hello entity is not healthy), and children health statistics (when applicable).</span></span>

> [!NOTE]
> <span data-ttu-id="b73be-134">En entitet hälsa returneras när den är helt fylld i hello health store.</span><span class="sxs-lookup"><span data-stu-id="b73be-134">A health entity is returned when it is fully populated in hello health store.</span></span> <span data-ttu-id="b73be-135">hello enheten måste vara aktiva (inte tas bort) och har en system-rapport.</span><span class="sxs-lookup"><span data-stu-id="b73be-135">hello entity must be active (not deleted) and have a system report.</span></span> <span data-ttu-id="b73be-136">Dess överordnade entiteter på hello hierarkin kedja måste också ha systemrapporter.</span><span class="sxs-lookup"><span data-stu-id="b73be-136">Its parent entities on hello hierarchy chain must also have system reports.</span></span> <span data-ttu-id="b73be-137">Om någon av dessa villkor inte är uppfyllda hello hälsa frågar returnera en [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) med [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` som visar varför hello entitet returneras inte.</span><span class="sxs-lookup"><span data-stu-id="b73be-137">If any of these conditions are not satisfied, hello health queries return a [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) with [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` that shows why hello entity is not returned.</span></span>
>
>

<span data-ttu-id="b73be-138">Hej hälsoförfrågningar måste klara i hello entitet identifierare som är beroende av hello entitetstypen.</span><span class="sxs-lookup"><span data-stu-id="b73be-138">hello health queries must pass in hello entity identifier, which depends on hello entity type.</span></span> <span data-ttu-id="b73be-139">hello frågor accepterar valfria hälsa principparametrar.</span><span class="sxs-lookup"><span data-stu-id="b73be-139">hello queries accept optional health policy parameters.</span></span> <span data-ttu-id="b73be-140">Om inga hälsoprinciper anges hello [hälsoprinciper](service-fabric-health-introduction.md#health-policies) från hello klustret eller programmanifestet används för utvärdering.</span><span class="sxs-lookup"><span data-stu-id="b73be-140">If no health policies are specified, hello [health policies](service-fabric-health-introduction.md#health-policies) from hello cluster or application manifest are used for evaluation.</span></span> <span data-ttu-id="b73be-141">Om hello manifesten får inte innehålla en definition för hälsoprinciper, som hello standard hälsoprinciper används för utvärdering.</span><span class="sxs-lookup"><span data-stu-id="b73be-141">If hello manifests don't contain a definition for health policies, hello default health policies are used for evaluation.</span></span> <span data-ttu-id="b73be-142">hello standard hälsoprinciper inte tolererar att ett fel.</span><span class="sxs-lookup"><span data-stu-id="b73be-142">hello default health policies do not tolerate any failures.</span></span> <span data-ttu-id="b73be-143">hello frågor även godta filter för att returnera endast delvis underordnade objekt eller händelser--hello dem som respektera hello angivna filtren har använts.</span><span class="sxs-lookup"><span data-stu-id="b73be-143">hello queries also accept filters for returning only partial children or events--hello ones that respect hello specified filters.</span></span> <span data-ttu-id="b73be-144">Ett annat filter kan exklusive hello underordnade statistik.</span><span class="sxs-lookup"><span data-stu-id="b73be-144">Another filter allows excluding hello children statistics.</span></span>

> [!NOTE]
> <span data-ttu-id="b73be-145">hello utdatafilter tillämpas på hello serversidan så hello meddelandestorlek svar minskas.</span><span class="sxs-lookup"><span data-stu-id="b73be-145">hello output filters are applied on hello server side, so hello message reply size is reduced.</span></span> <span data-ttu-id="b73be-146">Vi rekommenderar att du använder hello utdatafilter toolimit hello data returneras i stället använda filter i hello på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="b73be-146">We recommended that you use hello output filters toolimit hello data returned, rather than apply filters on hello client side.</span></span>
>
>

<span data-ttu-id="b73be-147">En entitets hälsotillstånd innehåller:</span><span class="sxs-lookup"><span data-stu-id="b73be-147">An entity's health contains:</span></span>

* <span data-ttu-id="b73be-148">hello samman hälsotillståndet för hello entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-148">hello aggregated health state of hello entity.</span></span> <span data-ttu-id="b73be-149">Beräknad av hello health store baserat på entiteten hälsorapporter, underordnade hälsotillstånd (i förekommande fall) och hälsoprinciper.</span><span class="sxs-lookup"><span data-stu-id="b73be-149">Computed by hello health store based on entity health reports, child health states (when applicable), and health policies.</span></span> <span data-ttu-id="b73be-150">Läs mer om [entiteten hälsoutvärderingen](service-fabric-health-introduction.md#health-evaluation).</span><span class="sxs-lookup"><span data-stu-id="b73be-150">Read more about [entity health evaluation](service-fabric-health-introduction.md#health-evaluation).</span></span>  
* <span data-ttu-id="b73be-151">hello hälsa händelser på hello entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-151">hello health events on hello entity.</span></span>
* <span data-ttu-id="b73be-152">hello insamling av hälsotillstånd för alla underordnade för hello entiteter som kan ha underordnade.</span><span class="sxs-lookup"><span data-stu-id="b73be-152">hello collection of health states of all children for hello entities that can have children.</span></span> <span data-ttu-id="b73be-153">hello hälsotillstånd innehåller entitet identifierare och hello aggregerade hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-153">hello health states contain entity identifiers and hello aggregated health state.</span></span> <span data-ttu-id="b73be-154">tooget hela hälsotillståndet för ett barn anropa hello frågan hälsa för hello underordnade entitetstypen och ange hello underordnade identifierare.</span><span class="sxs-lookup"><span data-stu-id="b73be-154">tooget complete health for a child, call hello query health for hello child entity type and pass in hello child identifier.</span></span>
* <span data-ttu-id="b73be-155">hello ohälsosamt utvärderingar den punkt toohello rapportera som utlöste hello tillståndet för hello entiteten om hello entiteten inte är felfri.</span><span class="sxs-lookup"><span data-stu-id="b73be-155">hello unhealthy evaluations that point toohello report that triggered hello state of hello entity, if hello entity is not healthy.</span></span> <span data-ttu-id="b73be-156">hello-utvärderingar är rekursiv, som innehåller hello underordnade hälsa utvärderingar som utlöste aktuellt hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-156">hello evaluations are recursive, containing hello children health evaluations that triggered current health state.</span></span> <span data-ttu-id="b73be-157">Till exempel rapporterade en watchdog ett fel mot en replik.</span><span class="sxs-lookup"><span data-stu-id="b73be-157">For example, a watchdog reported an error against a replica.</span></span> <span data-ttu-id="b73be-158">Hej programhälsan visar en ohälsosamt utvärdering på grund av feltillstånd tooan-tjänst. hello-tjänsten är i feltillstånd på grund av tooa partition i fel. hello partitionen är i feltillstånd på grund av tooa replik i fel. hello replik är felfri på grund av hälsorapport för toohello watchdog fel.</span><span class="sxs-lookup"><span data-stu-id="b73be-158">hello application health shows an unhealthy evaluation due tooan unhealthy service; hello service is unhealthy due tooa partition in error; hello partition is unhealthy due tooa replica in error; hello replica is unhealthy due toohello watchdog error health report.</span></span>
* <span data-ttu-id="b73be-159">hello hälsostatistik för alla underordnade typer av hello entiteter som har underordnade.</span><span class="sxs-lookup"><span data-stu-id="b73be-159">hello health statistics for all children types of hello entities that have children.</span></span> <span data-ttu-id="b73be-160">Kluster visar hello Totalt antal program, tjänster, partitioner, repliker och distribuerats entiteter i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="b73be-160">For example, cluster health shows hello total number of applications, services, partitions, replicas, and deployed entities in hello cluster.</span></span> <span data-ttu-id="b73be-161">Tjänstens hälsa visar hello totala antalet partitioner och repliker under hello angivna tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b73be-161">Service health shows hello total number of partitions and replicas under hello specified service.</span></span>

## <a name="get-cluster-health"></a><span data-ttu-id="b73be-162">Hämta kluster</span><span class="sxs-lookup"><span data-stu-id="b73be-162">Get cluster health</span></span>
<span data-ttu-id="b73be-163">Returnerar hello hälsotillstånd hello klustret entiteten och innehåller hello hälsotillstånd för program och noder (underordnade hello kluster).</span><span class="sxs-lookup"><span data-stu-id="b73be-163">Returns hello health of hello cluster entity and contains hello health states of applications and nodes (children of hello cluster).</span></span> <span data-ttu-id="b73be-164">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-164">Input:</span></span>

* <span data-ttu-id="b73be-165">[Valfritt] hello klustret hälsoprincip för tooevaluate hello noder och hello klusterhändelser.</span><span class="sxs-lookup"><span data-stu-id="b73be-165">[Optional] hello cluster health policy used tooevaluate hello nodes and hello cluster events.</span></span>
* <span data-ttu-id="b73be-166">[Valfritt] hello hälsa princip programavbildningen, används hello hälsoprinciper toooverride hello application manifest principer.</span><span class="sxs-lookup"><span data-stu-id="b73be-166">[Optional] hello application health policy map, with hello health policies used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="b73be-167">[Valfritt] Filter för händelser, noder och program som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel).</span><span class="sxs-lookup"><span data-stu-id="b73be-167">[Optional] Filters for events, nodes, and applications that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-168">Alla händelser, noder och program är används tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-168">All events, nodes, and applications are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="b73be-169">[Valfritt] Filtrera tooexclude hälsostatistik.</span><span class="sxs-lookup"><span data-stu-id="b73be-169">[Optional] Filter tooexclude health statistics.</span></span>
* <span data-ttu-id="b73be-170">[Valfritt] Filtrera tooinclude fabric: / System hälsostatistik i hello hälsostatistik.</span><span class="sxs-lookup"><span data-stu-id="b73be-170">[Optional] Filter tooinclude fabric:/System health statistics in hello health statistics.</span></span> <span data-ttu-id="b73be-171">Gäller endast när hello hälsostatistik inte utesluts.</span><span class="sxs-lookup"><span data-stu-id="b73be-171">Only applicable when hello health statistics are not excluded.</span></span> <span data-ttu-id="b73be-172">Som standard innehåller hello hälsostatistik bara statistik för inte hello System-program och program.</span><span class="sxs-lookup"><span data-stu-id="b73be-172">By default, hello health statistics include only statistics for user applications and not hello System application.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-173">API</span><span class="sxs-lookup"><span data-stu-id="b73be-173">API</span></span>
<span data-ttu-id="b73be-174">tooget hälsa bör du skapa en `FabricClient` och anrop hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) metod på dess **HealthManager**.</span><span class="sxs-lookup"><span data-stu-id="b73be-174">tooget cluster health, create a `FabricClient` and call hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) method on its **HealthManager**.</span></span>

<span data-ttu-id="b73be-175">hello hämtar följande anrop hello klustret hälsotillstånd:</span><span class="sxs-lookup"><span data-stu-id="b73be-175">hello following call gets hello cluster health:</span></span>

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

<span data-ttu-id="b73be-176">hello följande kod hämtar hello klustret hälsa genom att använda en anpassad klustret hälsoprincip och filtrerar för noder och program.</span><span class="sxs-lookup"><span data-stu-id="b73be-176">hello following code gets hello cluster health by using a custom cluster health policy and filters for nodes and applications.</span></span> <span data-ttu-id="b73be-177">Det anger att hello hälsostatistik inkluderar hello fabric: / statistik för filsystemet.</span><span class="sxs-lookup"><span data-stu-id="b73be-177">It specifies that hello health statistics include hello fabric:/System statistics.</span></span> <span data-ttu-id="b73be-178">Den skapar [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), som innehåller information om hello.</span><span class="sxs-lookup"><span data-stu-id="b73be-178">It creates [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), which contains hello input information.</span></span>

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="b73be-179">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-179">PowerShell</span></span>
<span data-ttu-id="b73be-180">hello cmdlet tooget hello klustret hälsa är [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth).</span><span class="sxs-lookup"><span data-stu-id="b73be-180">hello cmdlet tooget hello cluster health is [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth).</span></span> <span data-ttu-id="b73be-181">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-181">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="b73be-182">hello hello klustret är fem noder och hello systemprogram fabric: / WordCount konfigurerats enligt beskrivningen.</span><span class="sxs-lookup"><span data-stu-id="b73be-182">hello state of hello cluster is five nodes, hello system application, and fabric:/WordCount configured as described.</span></span>

<span data-ttu-id="b73be-183">hello följande cmdlet hämtar kluster med hjälp av standard hälsoprinciper.</span><span class="sxs-lookup"><span data-stu-id="b73be-183">hello following cmdlet gets cluster health by using default health policies.</span></span> <span data-ttu-id="b73be-184">hello aggregerade hälsotillstånd är en varning, eftersom hello fabric: / WordCount-program finns i varningen.</span><span class="sxs-lookup"><span data-stu-id="b73be-184">hello aggregated health state is warning, because hello fabric:/WordCount application is in warning.</span></span> <span data-ttu-id="b73be-185">Observera hur hello ohälsosamt utvärderingar tillhandahåller information om hello villkor som utlöste hello samman hälsa.</span><span class="sxs-lookup"><span data-stu-id="b73be-185">Note how hello unhealthy evaluations provide details on hello conditions that triggered hello aggregated health.</span></span>

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                          
                          
NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_0
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

<span data-ttu-id="b73be-186">hello hämtar följande PowerShell-cmdlet hello hälsotillstånd hello kluster med hjälp av en princip för anpassade program.</span><span class="sxs-lookup"><span data-stu-id="b73be-186">hello following PowerShell cmdlet gets hello health of hello cluster by using a custom application policy.</span></span> <span data-ttu-id="b73be-187">Den filtrerar resultaten tooget endast program och noderna i felet eller varningen.</span><span class="sxs-lookup"><span data-stu-id="b73be-187">It filters results tooget only applications and nodes in error or warning.</span></span> <span data-ttu-id="b73be-188">Därför kan returneras inga noder, som de är alla felfritt.</span><span class="sxs-lookup"><span data-stu-id="b73be-188">As a result, no nodes are returned, as they are all healthy.</span></span> <span data-ttu-id="b73be-189">Endast hello fabric: / WordCount programmet respekterar hello program filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-189">Only hello fabric:/WordCount application respects hello applications filter.</span></span> <span data-ttu-id="b73be-190">Eftersom hello anpassad princip anger tooconsider varningar som fel för hello fabric: / WordCount programmet hello programmet utvärderas som fel och är därför hello klustret.</span><span class="sxs-lookup"><span data-stu-id="b73be-190">Because hello custom policy specifies tooconsider warnings as errors for hello fabric:/WordCount application, hello application is evaluated as in error, and so is hello cluster.</span></span>

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                          
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="rest"></a><span data-ttu-id="b73be-191">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-191">REST</span></span>
<span data-ttu-id="b73be-192">Du kan hämta klustret hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-192">You can get cluster health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-node-health"></a><span data-ttu-id="b73be-193">Hämta nod hälsa</span><span class="sxs-lookup"><span data-stu-id="b73be-193">Get node health</span></span>
<span data-ttu-id="b73be-194">Returnerar hello hälsotillståndet för en nod entitet och innehåller hello hälsa händelser som rapporterats för hello nod.</span><span class="sxs-lookup"><span data-stu-id="b73be-194">Returns hello health of a node entity and contains hello health events reported on hello node.</span></span> <span data-ttu-id="b73be-195">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-195">Input:</span></span>

* <span data-ttu-id="b73be-196">[Krävs] hello nodnamn som identifierar hello-nod.</span><span class="sxs-lookup"><span data-stu-id="b73be-196">[Required] hello node name that identifies hello node.</span></span>
* <span data-ttu-id="b73be-197">[Valfritt] hello klustret hälsa principinställningar används tooevaluate hälsa.</span><span class="sxs-lookup"><span data-stu-id="b73be-197">[Optional] hello cluster health policy settings used tooevaluate health.</span></span>
* <span data-ttu-id="b73be-198">[Valfritt] Filter för händelser som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel).</span><span class="sxs-lookup"><span data-stu-id="b73be-198">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-199">Alla händelser har använt tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-199">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-200">API</span><span class="sxs-lookup"><span data-stu-id="b73be-200">API</span></span>
<span data-ttu-id="b73be-201">tooget noden hälsa genom hello API, skapa en `FabricClient` och anrop hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) metod på dess HealthManager.</span><span class="sxs-lookup"><span data-stu-id="b73be-201">tooget node health through hello API, create a `FabricClient` and call hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="b73be-202">hello hämtar följande kod hello nod hälsan för hello angivna nodnamn:</span><span class="sxs-lookup"><span data-stu-id="b73be-202">hello following code gets hello node health for hello specified node name:</span></span>

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

<span data-ttu-id="b73be-203">hello följande kod hämtar hello nod hälsa för hello angivna nodnamnet och överför i händelsefilter och anpassad princip via [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):</span><span class="sxs-lookup"><span data-stu-id="b73be-203">hello following code gets hello node health for hello specified node name and passes in events filter and custom policy through [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):</span></span>

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="b73be-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-204">PowerShell</span></span>
<span data-ttu-id="b73be-205">hello cmdlet tooget hello nod hälsa är [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth).</span><span class="sxs-lookup"><span data-stu-id="b73be-205">hello cmdlet tooget hello node health is [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth).</span></span> <span data-ttu-id="b73be-206">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-206">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>
<span data-ttu-id="b73be-207">hello följande cmdlet hämtar hello nod hälsa genom att använda standard hälsoprinciper:</span><span class="sxs-lookup"><span data-stu-id="b73be-207">hello following cmdlet gets hello node health by using default health policies:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

<span data-ttu-id="b73be-208">hello hämtar följande cmdlet hello hälsotillståndet för alla noder i klustret hello:</span><span class="sxs-lookup"><span data-stu-id="b73be-208">hello following cmdlet gets hello health of all nodes in hello cluster:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a><span data-ttu-id="b73be-209">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-209">REST</span></span>
<span data-ttu-id="b73be-210">Du kan hämta nod hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-210">You can get node health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-application-health"></a><span data-ttu-id="b73be-211">Hämta programmets hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="b73be-211">Get application health</span></span>
<span data-ttu-id="b73be-212">Returnerar hello hälsa för en entitet i programmet.</span><span class="sxs-lookup"><span data-stu-id="b73be-212">Returns hello health of an application entity.</span></span> <span data-ttu-id="b73be-213">Den innehåller hello hälsotillstånd hello distribuerade program och tjänsten underordnade.</span><span class="sxs-lookup"><span data-stu-id="b73be-213">It contains hello health states of hello deployed application and service children.</span></span> <span data-ttu-id="b73be-214">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-214">Input:</span></span>

* <span data-ttu-id="b73be-215">[Krävs] hello programnamnet (URI) som identifierar hello program.</span><span class="sxs-lookup"><span data-stu-id="b73be-215">[Required] hello application name (URI) that identifies hello application.</span></span>
* <span data-ttu-id="b73be-216">[Valfritt] hello programmets hälsoprincip används toooverride hello application manifest principer.</span><span class="sxs-lookup"><span data-stu-id="b73be-216">[Optional] hello application health policy used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="b73be-217">[Valfritt] Filter för händelser, tjänster och distribuerade program som anger vilka poster returneras i hello resultatet (till exempel fel, eller både varningar och fel) är av intresse.</span><span class="sxs-lookup"><span data-stu-id="b73be-217">[Optional] Filters for events, services, and deployed applications that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-218">Alla händelser, tjänster och distribuerade program kan du använda tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-218">All events, services, and deployed applications are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="b73be-219">[Valfritt] Filtrera tooexclude hello hälsostatistik.</span><span class="sxs-lookup"><span data-stu-id="b73be-219">[Optional] Filter tooexclude hello health statistics.</span></span> <span data-ttu-id="b73be-220">Om inget anges hello hälsostatistik inkluderar hello ok, varning och antal fel för alla program barn: tjänster, partitioner, repliker, distribuerade program och distribueras servicepaket.</span><span class="sxs-lookup"><span data-stu-id="b73be-220">If not specified, hello health statistics include hello ok, warning, and error count for all application children: services, partitions, replicas, deployed applications, and deployed service packages.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-221">API</span><span class="sxs-lookup"><span data-stu-id="b73be-221">API</span></span>
<span data-ttu-id="b73be-222">tooget programmets hälsotillstånd, skapa en `FabricClient` och anrop hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) metod på dess HealthManager.</span><span class="sxs-lookup"><span data-stu-id="b73be-222">tooget application health, create a `FabricClient` and call hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="b73be-223">hello hämtar följande kod hello programhälsan hello angivna programnamnet (URI):</span><span class="sxs-lookup"><span data-stu-id="b73be-223">hello following code gets hello application health for hello specified application name (URI):</span></span>

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

<span data-ttu-id="b73be-224">hello följande kod hämtar hello programhälsan hello angivna programnamnet (URI) med filter och anpassade principer som angetts via [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="b73be-224">hello following code gets hello application health for hello specified application name (URI), with filters and custom policies specified via [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).</span></span>

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="b73be-225">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-225">PowerShell</span></span>
<span data-ttu-id="b73be-226">hello cmdlet tooget hello programhälsan är [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="b73be-226">hello cmdlet tooget hello application health is [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps).</span></span> <span data-ttu-id="b73be-227">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-227">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="b73be-228">hello följande cmdlet returnerar hello hälsotillstånd hello **fabric: / WordCount** program:</span><span class="sxs-lookup"><span data-stu-id="b73be-228">hello following cmdlet returns hello health of hello **fabric:/WordCount** application:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok
                                  
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning
                                  
DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok
                                  
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
                                  
HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

<span data-ttu-id="b73be-229">Hej följande PowerShell-cmdlet överför i anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="b73be-229">hello following PowerShell cmdlet passes in custom policies.</span></span> <span data-ttu-id="b73be-230">Den filtrerar underordnade och händelser.</span><span class="sxs-lookup"><span data-stu-id="b73be-230">It also filters children and events.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error
                                  
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a><span data-ttu-id="b73be-231">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-231">REST</span></span>
<span data-ttu-id="b73be-232">Du kan hämta programmets hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-232">You can get application health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-service-health"></a><span data-ttu-id="b73be-233">Hämta tjänstens hälsa</span><span class="sxs-lookup"><span data-stu-id="b73be-233">Get service health</span></span>
<span data-ttu-id="b73be-234">Returnerar hello hälsotillståndet för en tjänst-enhet.</span><span class="sxs-lookup"><span data-stu-id="b73be-234">Returns hello health of a service entity.</span></span> <span data-ttu-id="b73be-235">Den innehåller hello partition hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-235">It contains hello partition health states.</span></span> <span data-ttu-id="b73be-236">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-236">Input:</span></span>

* <span data-ttu-id="b73be-237">[Krävs] hello tjänstnamn (URI) som identifierar hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="b73be-237">[Required] hello service name (URI) that identifies hello service.</span></span>
* <span data-ttu-id="b73be-238">[Valfritt] hello programmets hälsoprincip används toooverride hello application manifest princip.</span><span class="sxs-lookup"><span data-stu-id="b73be-238">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="b73be-239">[Valfritt] Filter för händelser och partitioner som anger vilka poster returneras i hello resultatet (till exempel fel, eller både varningar och fel) är av intresse.</span><span class="sxs-lookup"><span data-stu-id="b73be-239">[Optional] Filters for events and partitions that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-240">Alla händelser och partitioner är används tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-240">All events and partitions are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="b73be-241">[Valfritt] Filtrera tooexclude hälsostatistik.</span><span class="sxs-lookup"><span data-stu-id="b73be-241">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="b73be-242">Om inget anges Hej hälsa statistik visar hello ok, varning och fel antal för alla partitioner och repliker av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b73be-242">If not specified, hello health statistics show hello ok, warning, and error count for all partitions and replicas of hello service.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-243">API</span><span class="sxs-lookup"><span data-stu-id="b73be-243">API</span></span>
<span data-ttu-id="b73be-244">tjänstens hälsa för tooget via hello API, skapa en `FabricClient` och anrop hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) metod på dess HealthManager.</span><span class="sxs-lookup"><span data-stu-id="b73be-244">tooget service health through hello API, create a `FabricClient` and call hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="b73be-245">hello hämtar följande exempel hello hälsotillståndet för en tjänst med angivet tjänstnamn (URI):</span><span class="sxs-lookup"><span data-stu-id="b73be-245">hello following example gets hello health of a service with specified service name (URI):</span></span>

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

<span data-ttu-id="b73be-246">hello följande kod hämtar hello tjänstens hälsa för hello angivet tjänstnamn (URI), ange filter och anpassade principer via [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):</span><span class="sxs-lookup"><span data-stu-id="b73be-246">hello following code gets hello service health for hello specified service name (URI), specifying filters and custom policy via [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):</span></span>

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="b73be-247">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-247">PowerShell</span></span>
<span data-ttu-id="b73be-248">tjänstens hälsa för hello cmdlet tooget hello är [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth).</span><span class="sxs-lookup"><span data-stu-id="b73be-248">hello cmdlet tooget hello service health is [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth).</span></span> <span data-ttu-id="b73be-249">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-249">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="b73be-250">hello följande cmdlet hämtar hello tjänstens hälsa med hjälp av standard hälsoprinciper:</span><span class="sxs-lookup"><span data-stu-id="b73be-250">hello following cmdlet gets hello service health by using default health policies:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                        
                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                        
                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="b73be-251">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-251">REST</span></span>
<span data-ttu-id="b73be-252">Du kan hämta tjänstens hälsa med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-252">You can get service health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-partition-health"></a><span data-ttu-id="b73be-253">Hämta partitionen hälsa</span><span class="sxs-lookup"><span data-stu-id="b73be-253">Get partition health</span></span>
<span data-ttu-id="b73be-254">Returnerar hello hälsotillståndet för en partition entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-254">Returns hello health of a partition entity.</span></span> <span data-ttu-id="b73be-255">Den innehåller hello replik hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-255">It contains hello replica health states.</span></span> <span data-ttu-id="b73be-256">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-256">Input:</span></span>

* <span data-ttu-id="b73be-257">[Krävs] hello partition ID (GUID) som identifierar hello partition.</span><span class="sxs-lookup"><span data-stu-id="b73be-257">[Required] hello partition ID (GUID) that identifies hello partition.</span></span>
* <span data-ttu-id="b73be-258">[Valfritt] hello programmets hälsoprincip används toooverride hello application manifest princip.</span><span class="sxs-lookup"><span data-stu-id="b73be-258">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="b73be-259">[Valfritt] Filter för händelser och repliker som anger vilka poster returneras i hello resultatet (till exempel fel, eller både varningar och fel) är av intresse.</span><span class="sxs-lookup"><span data-stu-id="b73be-259">[Optional] Filters for events and replicas that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-260">Alla händelser och repliker är används tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-260">All events and replicas are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="b73be-261">[Valfritt] Filtrera tooexclude hälsostatistik.</span><span class="sxs-lookup"><span data-stu-id="b73be-261">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="b73be-262">Om inget annat anges, visar hello hälsostatistik hur många repliker är i ok, varning och fel tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-262">If not specified, hello health statistics show how many replicas are in ok, warning, and error states.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-263">API</span><span class="sxs-lookup"><span data-stu-id="b73be-263">API</span></span>
<span data-ttu-id="b73be-264">tooget partition hälsa genom hello API, skapa en `FabricClient` och anrop hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) metod på dess HealthManager.</span><span class="sxs-lookup"><span data-stu-id="b73be-264">tooget partition health through hello API, create a `FabricClient` and call hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) method on its HealthManager.</span></span> <span data-ttu-id="b73be-265">toospecify valfria parametrar, skapa [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="b73be-265">toospecify optional parameters, create [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).</span></span>

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a><span data-ttu-id="b73be-266">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-266">PowerShell</span></span>
<span data-ttu-id="b73be-267">hello cmdlet tooget hello partition hälsa är [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth).</span><span class="sxs-lookup"><span data-stu-id="b73be-267">hello cmdlet tooget hello partition health is [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth).</span></span> <span data-ttu-id="b73be-268">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-268">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="b73be-269">hello följande cmdlet hämtar hello hälsotillstånd för alla partitioner i hello **fabric: / WordCount/WordCountService** tjänsten och filtrerar bort repliken hälsotillstånd:</span><span class="sxs-lookup"><span data-stu-id="b73be-269">hello following cmdlet gets hello health for all partitions of hello **fabric:/WordCount/WordCountService** service and filters out replica health states:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer was unable toofind a placement for one or more of hello Service's Replicas:
                        Secondary replica could not be placed due toohello following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="b73be-270">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-270">REST</span></span>
<span data-ttu-id="b73be-271">Du kan hämta partitionen hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-271">You can get partition health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-replica-health"></a><span data-ttu-id="b73be-272">Hämta replik hälsa</span><span class="sxs-lookup"><span data-stu-id="b73be-272">Get replica health</span></span>
<span data-ttu-id="b73be-273">Returnerar hello hälsotillståndet för en tillståndskänslig service replik eller en tillståndslös tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="b73be-273">Returns hello health of a stateful service replica or a stateless service instance.</span></span> <span data-ttu-id="b73be-274">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-274">Input:</span></span>

* <span data-ttu-id="b73be-275">[Krävs] hello partitions ID (GUID) och replik-ID som identifierar hello replik.</span><span class="sxs-lookup"><span data-stu-id="b73be-275">[Required] hello partition ID (GUID) and replica ID that identifies hello replica.</span></span>
* <span data-ttu-id="b73be-276">[Valfritt] hello applikationsparametrarna hälsa principen används toooverride hello application manifest principer.</span><span class="sxs-lookup"><span data-stu-id="b73be-276">[Optional] hello application health policy parameters used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="b73be-277">[Valfritt] Filter för händelser som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel).</span><span class="sxs-lookup"><span data-stu-id="b73be-277">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-278">Alla händelser har använt tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-278">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-279">API</span><span class="sxs-lookup"><span data-stu-id="b73be-279">API</span></span>
<span data-ttu-id="b73be-280">tooget hello replik hälsa genom hello API, skapa en `FabricClient` och anrop hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) metod på dess HealthManager.</span><span class="sxs-lookup"><span data-stu-id="b73be-280">tooget hello replica health through hello API, create a `FabricClient` and call hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) method on its HealthManager.</span></span> <span data-ttu-id="b73be-281">toospecify avancerade parametrar, Använd [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="b73be-281">toospecify advanced parameters, use [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).</span></span>

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a><span data-ttu-id="b73be-282">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-282">PowerShell</span></span>
<span data-ttu-id="b73be-283">hello cmdlet tooget hello replik hälsa är [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth).</span><span class="sxs-lookup"><span data-stu-id="b73be-283">hello cmdlet tooget hello replica health is [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth).</span></span> <span data-ttu-id="b73be-284">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-284">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="b73be-285">hello hämtar följande cmdlet hello hälsotillstånd hello primära repliken för alla partitioner i hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="b73be-285">hello following cmdlet gets hello health of hello primary replica for all partitions of hello service:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a><span data-ttu-id="b73be-286">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-286">REST</span></span>
<span data-ttu-id="b73be-287">Du kan hämta replik hälsotillstånd med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-287">You can get replica health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-deployed-application-health"></a><span data-ttu-id="b73be-288">Hämta distribuerade programhälsan</span><span class="sxs-lookup"><span data-stu-id="b73be-288">Get deployed application health</span></span>
<span data-ttu-id="b73be-289">Returnerar hello hälsotillståndet för ett program som distribuerats på en nod entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-289">Returns hello health of an application deployed on a node entity.</span></span> <span data-ttu-id="b73be-290">Den innehåller hello distribuerade tjänsten paketet hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-290">It contains hello deployed service package health states.</span></span> <span data-ttu-id="b73be-291">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-291">Input:</span></span>

* <span data-ttu-id="b73be-292">[Krävs] hello programnamn (URI) och nodnamnet (sträng) som identifierar hello distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="b73be-292">[Required] hello application name (URI) and node name (string) that identify hello deployed application.</span></span>
* <span data-ttu-id="b73be-293">[Valfritt] hello programmets hälsoprincip används toooverride hello application manifest principer.</span><span class="sxs-lookup"><span data-stu-id="b73be-293">[Optional] hello application health policy used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="b73be-294">[Valfritt] Filter för händelser och distribuerade tjänstpaket som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel).</span><span class="sxs-lookup"><span data-stu-id="b73be-294">[Optional] Filters for events and deployed service packages that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-295">Alla händelser och distribuerade tjänstpaket är används tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-295">All events and deployed service packages are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="b73be-296">[Valfritt] Filtrera tooexclude hälsostatistik.</span><span class="sxs-lookup"><span data-stu-id="b73be-296">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="b73be-297">Om inget annat anges, visar hello hälsostatistik hello antal distribuerade tjänstpaket i ok, varning och fel hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-297">If not specified, hello health statistics show hello number of deployed service packages in ok, warning, and error health states.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-298">API</span><span class="sxs-lookup"><span data-stu-id="b73be-298">API</span></span>
<span data-ttu-id="b73be-299">tooget hello hälsotillståndet för ett program som distribuerats på en nod via hello API, skapa en `FabricClient` och anrop hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) metod på dess HealthManager.</span><span class="sxs-lookup"><span data-stu-id="b73be-299">tooget hello health of an application deployed on a node through hello API, create a `FabricClient` and call hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) method on its HealthManager.</span></span> <span data-ttu-id="b73be-300">toospecify valfria parametrar, använda [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="b73be-300">toospecify optional parameters, use [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).</span></span>

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a><span data-ttu-id="b73be-301">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-301">PowerShell</span></span>
<span data-ttu-id="b73be-302">hello cmdlet tooget hello distribuerade programhälsan är [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="b73be-302">hello cmdlet tooget hello deployed application health is [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps).</span></span> <span data-ttu-id="b73be-303">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-303">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="b73be-304">toofind ut där ett program har distribuerats, kör [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) och titta på hello distribuerade program underordnade.</span><span class="sxs-lookup"><span data-stu-id="b73be-304">toofind out where an application is deployed, run [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) and look at hello deployed application children.</span></span>

<span data-ttu-id="b73be-305">hello följande cmdlet hämtar hello hälsotillstånd hello **fabric: / WordCount** program som distribuerats på **_Node_2**.</span><span class="sxs-lookup"><span data-stu-id="b73be-305">hello following cmdlet gets hello health of hello **fabric:/WordCount** application deployed on **_Node_2**.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM
                                     
HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="b73be-306">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-306">REST</span></span>
<span data-ttu-id="b73be-307">Du kan hämta distribuerade programhälsan med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-307">You can get deployed application health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-deployed-service-package-health"></a><span data-ttu-id="b73be-308">Hämta distribuerade tjänstens hälsa för paketet</span><span class="sxs-lookup"><span data-stu-id="b73be-308">Get deployed service package health</span></span>
<span data-ttu-id="b73be-309">Returnerar hello hälsotillståndet för en distribuerad tjänst paketet entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-309">Returns hello health of a deployed service package entity.</span></span> <span data-ttu-id="b73be-310">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-310">Input:</span></span>

* <span data-ttu-id="b73be-311">[Krävs] hello programnamn (URI), nodnamnet (sträng) och manifestet tjänstnamn (sträng) som identifierar hello distribueras tjänstepaketet.</span><span class="sxs-lookup"><span data-stu-id="b73be-311">[Required] hello application name (URI), node name (string), and service manifest name (string) that identify hello deployed service package.</span></span>
* <span data-ttu-id="b73be-312">[Valfritt] hello programmets hälsoprincip används toooverride hello application manifest princip.</span><span class="sxs-lookup"><span data-stu-id="b73be-312">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="b73be-313">[Valfritt] Filter för händelser som anger vilka poster som är intressanta och returneras i hello resultatet (till exempel fel, eller både varningar och fel).</span><span class="sxs-lookup"><span data-stu-id="b73be-313">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="b73be-314">Alla händelser har använt tooevaluate hello samman entitetshälsa, oavsett hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-314">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-315">API</span><span class="sxs-lookup"><span data-stu-id="b73be-315">API</span></span>
<span data-ttu-id="b73be-316">tooget hello hälsotillståndet för en distribuerad tjänstpaket via hello API, skapa en `FabricClient` och anrop hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) metod på dess HealthManager.</span><span class="sxs-lookup"><span data-stu-id="b73be-316">tooget hello health of a deployed service package through hello API, create a `FabricClient` and call hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) method on its HealthManager.</span></span> <span data-ttu-id="b73be-317">toospecify valfria parametrar, använda [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="b73be-317">toospecify optional parameters, use [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).</span></span>

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a><span data-ttu-id="b73be-318">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-318">PowerShell</span></span>
<span data-ttu-id="b73be-319">hello cmdlet tooget hello distribueras tjänstens paketet hälsa är [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth).</span><span class="sxs-lookup"><span data-stu-id="b73be-319">hello cmdlet tooget hello deployed service package health is [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth).</span></span> <span data-ttu-id="b73be-320">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-320">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="b73be-321">toosee där ett program har distribuerats, kör [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) och titta på hello distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="b73be-321">toosee where an application is deployed, run [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) and look at hello deployed applications.</span></span> <span data-ttu-id="b73be-322">toosee vars servicepaket finns i ett program, titta på hello distribuerade tjänsten paketet barn i hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) utdata.</span><span class="sxs-lookup"><span data-stu-id="b73be-322">toosee which service packages are in an application, look at hello deployed service package children in hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) output.</span></span>

<span data-ttu-id="b73be-323">hello följande cmdlet hämtar hello hälsotillstånd hello **WordCountServicePkg** tjänstpaket av hello **fabric: / WordCount** program som distribuerats på **_Node_2**.</span><span class="sxs-lookup"><span data-stu-id="b73be-323">hello following cmdlet gets hello health of hello **WordCountServicePkg** service package of hello **fabric:/WordCount** application deployed on **_Node_2**.</span></span> <span data-ttu-id="b73be-324">hello entiteten har **System.Hosting** rapporter för service-paket och startpunkten aktiveringen och typ av tjänst att registreringen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="b73be-324">hello entity has **System.Hosting** reports for successful service-package and entry-point activation, and successful service-type registration.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a><span data-ttu-id="b73be-325">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-325">REST</span></span>
<span data-ttu-id="b73be-326">Du kan hämta distribuerade tjänstens hälsa för paketet med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) som innehåller hälsoprinciper som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-326">You can get deployed service package health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="health-chunk-queries"></a><span data-ttu-id="b73be-327">Segmentet hälsoförfrågningar</span><span class="sxs-lookup"><span data-stu-id="b73be-327">Health chunk queries</span></span>
<span data-ttu-id="b73be-328">hello hälsoförfrågningar segment kan returnera flera nivåer klustret underordnade (rekursivt) per inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="b73be-328">hello health chunk queries can return multi-level cluster children (recursively), per input filters.</span></span> <span data-ttu-id="b73be-329">Den stöder avancerade filter som gör att många alternativ att välja hello underordnade toobe returneras.</span><span class="sxs-lookup"><span data-stu-id="b73be-329">It supports advanced filters that allow a lot of flexibility in choosing hello children toobe returned.</span></span> <span data-ttu-id="b73be-330">hello filter kan ange underordnade genom hello Unik identifierare eller andra gruppidentifierare och/eller hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-330">hello filters can specify children by hello unique identifier or by other group identifiers and/or health states.</span></span> <span data-ttu-id="b73be-331">Som standard ingår inga underordnade, som skillnad från toohealth kommandon som alltid innehåller underordnade på första nivån.</span><span class="sxs-lookup"><span data-stu-id="b73be-331">By default, no children are included, as opposed toohealth commands that always include first-level children.</span></span>

<span data-ttu-id="b73be-332">Hej [hälsoförfrågningar](service-fabric-view-entities-aggregated-health.md#health-queries) returnerar endast första nivån underordnade hello angetts entitet per krävs filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-332">hello [health queries](service-fabric-view-entities-aggregated-health.md#health-queries) return only first-level children of hello specified entity per required filters.</span></span> <span data-ttu-id="b73be-333">tooget hello underordnade hello underordnade, måste du anropa ytterligare hälsa API: er för varje entitet av intresse.</span><span class="sxs-lookup"><span data-stu-id="b73be-333">tooget hello children of hello children, you must call additional health APIs for each entity of interest.</span></span> <span data-ttu-id="b73be-334">På liknande sätt tooget hello hälsotillståndet för specifika enheter, måste du anropa en hälsa API för varje entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-334">Similarly, tooget hello health of specific entities, you must call one health API for each desired entity.</span></span> <span data-ttu-id="b73be-335">hello segmentet frågan avancerad filtrering kan du toorequest flera objekt av intresse för en fråga, minimera hello meddelandestorlek och hello antalet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b73be-335">hello chunk query advanced filtering allows you toorequest multiple items of interest in one query, minimizing hello message size and hello number of messages.</span></span>

<span data-ttu-id="b73be-336">hello-värdet för hello segmentet frågan är att du kan få hälsotillståndet för flera kluster entiteter (potentiellt alla kluster entiteter från krävs rot) i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="b73be-336">hello value of hello chunk query is that you can get health state for more cluster entities (potentially all cluster entities starting at required root) in one call.</span></span> <span data-ttu-id="b73be-337">Exempelvis kan du ange komplexa hälsa fråga:</span><span class="sxs-lookup"><span data-stu-id="b73be-337">You can express complex health query such as:</span></span>

* <span data-ttu-id="b73be-338">Returnerar endast program i fel och för dessa program innehåller alla tjänster i varning eller fel.</span><span class="sxs-lookup"><span data-stu-id="b73be-338">Return only applications in error, and for those applications include all services in warning or error.</span></span> <span data-ttu-id="b73be-339">Inkludera alla partitioner för returnerade tjänster.</span><span class="sxs-lookup"><span data-stu-id="b73be-339">For returned services, include all partitions.</span></span>
* <span data-ttu-id="b73be-340">Returnera endast hello hälsotillståndet hos fyra program som anges av deras namn.</span><span class="sxs-lookup"><span data-stu-id="b73be-340">Return only hello health of four applications, specified by their names.</span></span>
* <span data-ttu-id="b73be-341">Returnera endast hello hälsotillståndet hos program för en typ för önskat program.</span><span class="sxs-lookup"><span data-stu-id="b73be-341">Return only hello health of applications of a desired application type.</span></span>
* <span data-ttu-id="b73be-342">Returnera alla distribuerade enheter på en nod.</span><span class="sxs-lookup"><span data-stu-id="b73be-342">Return all deployed entities on a node.</span></span> <span data-ttu-id="b73be-343">Returnerar alla program, alla distribuerade program på hello nod och alla hello distribuerat tjänstpaket på noden.</span><span class="sxs-lookup"><span data-stu-id="b73be-343">Returns all applications, all deployed applications on hello specified node and all hello deployed service packages on that node.</span></span>
* <span data-ttu-id="b73be-344">Returnera alla repliker i fel.</span><span class="sxs-lookup"><span data-stu-id="b73be-344">Return all replicas in error.</span></span> <span data-ttu-id="b73be-345">Returnerar alla program, tjänster, partitioner och endast repliker i fel.</span><span class="sxs-lookup"><span data-stu-id="b73be-345">Returns all applications, services, partitions, and only replicas in error.</span></span>
* <span data-ttu-id="b73be-346">Returnera alla program.</span><span class="sxs-lookup"><span data-stu-id="b73be-346">Return all applications.</span></span> <span data-ttu-id="b73be-347">Inkludera alla partitioner för en angiven tjänst.</span><span class="sxs-lookup"><span data-stu-id="b73be-347">For a specified service, include all partitions.</span></span>

<span data-ttu-id="b73be-348">Hello hälsa segmentet frågan exponeras för närvarande endast för hello klustret entiteten.</span><span class="sxs-lookup"><span data-stu-id="b73be-348">Currently, hello health chunk query is exposed only for hello cluster entity.</span></span> <span data-ttu-id="b73be-349">Den returnerar ett kluster hälsa segment som innehåller:</span><span class="sxs-lookup"><span data-stu-id="b73be-349">It returns a cluster health chunk, which contains:</span></span>

* <span data-ttu-id="b73be-350">hello klustret samman hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-350">hello cluster aggregated health state.</span></span>
* <span data-ttu-id="b73be-351">hello hälsa tillstånd segmentet listan över noder som respektera inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="b73be-351">hello health state chunk list of nodes that respect input filters.</span></span>
* <span data-ttu-id="b73be-352">hello hälsa tillstånd segmentet lista över program som respektera inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="b73be-352">hello health state chunk list of applications that respect input filters.</span></span> <span data-ttu-id="b73be-353">Varje program hälsa tillstånd segment innehåller en segmentet lista med alla tjänster som respektera inkommande trafik och ett segment lista med alla distribuerade program som respektera hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-353">Each application health state chunk contains a chunk list with all services that respect input filters and a chunk list with all deployed applications that respect hello filters.</span></span> <span data-ttu-id="b73be-354">Samma för hello underordnade tjänster och distribuerade program.</span><span class="sxs-lookup"><span data-stu-id="b73be-354">Same for hello children of services and deployed applications.</span></span> <span data-ttu-id="b73be-355">På så sätt kan alla entiteter i hello klustret kan potentiellt returneras om det krävs hierarkiskt.</span><span class="sxs-lookup"><span data-stu-id="b73be-355">This way, all entities in hello cluster can be potentially returned if requested, in a hierarchical fashion.</span></span>

### <a name="cluster-health-chunk-query"></a><span data-ttu-id="b73be-356">Klustret hälsa segmentet fråga</span><span class="sxs-lookup"><span data-stu-id="b73be-356">Cluster health chunk query</span></span>
<span data-ttu-id="b73be-357">Returnerar hello hälsotillstånd hello klustret entiteten och innehåller hello hierarkiska hälsa tillstånd mängder krävs underordnade.</span><span class="sxs-lookup"><span data-stu-id="b73be-357">Returns hello health of hello cluster entity and contains hello hierarchical health state chunks of required children.</span></span> <span data-ttu-id="b73be-358">Indata:</span><span class="sxs-lookup"><span data-stu-id="b73be-358">Input:</span></span>

* <span data-ttu-id="b73be-359">[Valfritt] hello klustret hälsoprincip för tooevaluate hello noder och hello klusterhändelser.</span><span class="sxs-lookup"><span data-stu-id="b73be-359">[Optional] hello cluster health policy used tooevaluate hello nodes and hello cluster events.</span></span>
* <span data-ttu-id="b73be-360">[Valfritt] hello hälsa princip programavbildningen, används hello hälsoprinciper toooverride hello application manifest principer.</span><span class="sxs-lookup"><span data-stu-id="b73be-360">[Optional] hello application health policy map, with hello health policies used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="b73be-361">[Valfritt] Filter för noder och program som anger vilka transaktioner är intressanta och returneras i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="b73be-361">[Optional] Filters for nodes and applications that specify which entries are of interest and should be returned in hello result.</span></span> <span data-ttu-id="b73be-362">hello filter är särskilda tooan entitet/grupp av enheter eller tillämpliga tooall entiteter på den nivån.</span><span class="sxs-lookup"><span data-stu-id="b73be-362">hello filters are specific tooan entity/group of entities or are applicable tooall entities at that level.</span></span> <span data-ttu-id="b73be-363">hello listan över filter kan innehålla ett allmänt filter och/eller filter för specifika identifierare toofine grain entiteter som returneras av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="b73be-363">hello list of filters can contain one general filter and/or filters for specific identifiers toofine-grain entities returned by hello query.</span></span> <span data-ttu-id="b73be-364">Om den är tom returneras inte hello underordnade som standard.</span><span class="sxs-lookup"><span data-stu-id="b73be-364">If empty, hello children are not returned by default.</span></span>
  <span data-ttu-id="b73be-365">Läs mer om hello filter på [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) och [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter).</span><span class="sxs-lookup"><span data-stu-id="b73be-365">Read more about hello filters at [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) and [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter).</span></span> <span data-ttu-id="b73be-366">hello programmet filter kan rekursivt ange avancerade filter för barn.</span><span class="sxs-lookup"><span data-stu-id="b73be-366">hello application filters can recursively specify advanced filters for children.</span></span>

<span data-ttu-id="b73be-367">hello segmentet resultatet innehåller hello underordnade som respektera hello filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-367">hello chunk result includes hello children that respect hello filters.</span></span>

<span data-ttu-id="b73be-368">För närvarande returneras hello segmentet inte ohälsosamt utvärderingar eller entiteten händelser.</span><span class="sxs-lookup"><span data-stu-id="b73be-368">Currently, hello chunk query does not return unhealthy evaluations or entity events.</span></span> <span data-ttu-id="b73be-369">Den extra informationen kan hämtas med hjälp av hello befintliga kluster hälsa frågan.</span><span class="sxs-lookup"><span data-stu-id="b73be-369">That extra information can be obtained using hello existing cluster health query.</span></span>

### <a name="api"></a><span data-ttu-id="b73be-370">API</span><span class="sxs-lookup"><span data-stu-id="b73be-370">API</span></span>
<span data-ttu-id="b73be-371">tooget klustret hälsa dela in, skapa en `FabricClient` och anrop hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) metod på dess **HealthManager**.</span><span class="sxs-lookup"><span data-stu-id="b73be-371">tooget cluster health chunk, create a `FabricClient` and call hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) method on its **HealthManager**.</span></span> <span data-ttu-id="b73be-372">Du kan skicka in [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe hälsoprinciper och avancerade filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-372">You can pass in [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe health policies and advanced filters.</span></span>

<span data-ttu-id="b73be-373">hello hämtar följande kod klustret hälsa segment med avancerade filter.</span><span class="sxs-lookup"><span data-stu-id="b73be-373">hello following code gets cluster health chunk with advanced filters.</span></span>

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except hello ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="b73be-374">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b73be-374">PowerShell</span></span>
<span data-ttu-id="b73be-375">hello cmdlet tooget hello klustret hälsa är [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk).</span><span class="sxs-lookup"><span data-stu-id="b73be-375">hello cmdlet tooget hello cluster health is [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk).</span></span> <span data-ttu-id="b73be-376">Först ansluta toohello kluster med hjälp av hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b73be-376">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="b73be-377">hello hämtar följande kod noder endast om de är i fel utom en viss nod som alltid ska returneras.</span><span class="sxs-lookup"><span data-stu-id="b73be-377">hello following code gets nodes only if they are in Error except for a specific node, which should always be returned.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in hello cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1
                               
                               NodeName              : _Node_1
                               HealthState           : Ok
                               
ApplicationHealthStateChunks : None
```

<span data-ttu-id="b73be-378">hello följande cmdlet hämtar klustret segment med filter för programmet.</span><span class="sxs-lookup"><span data-stu-id="b73be-378">hello following cmdlet gets cluster chunk with application filters.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1
                               
                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1
                               
                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5
                               
                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

<span data-ttu-id="b73be-379">hello returnerar följande cmdlet alla distribuerade enheter på en nod.</span><span class="sxs-lookup"><span data-stu-id="b73be-379">hello following cmdlet returns all deployed entities on a node.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2
                               
                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
                               
                               
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a><span data-ttu-id="b73be-380">REST</span><span class="sxs-lookup"><span data-stu-id="b73be-380">REST</span></span>
<span data-ttu-id="b73be-381">Du kan hämta klustret hälsa segment med en [GET-begäran](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) eller en [POST-begäran](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) som innehåller hälsoprinciper och avancerade filter som beskrivs i hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="b73be-381">You can get cluster health chunk with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) that includes health policies and advanced filters described in hello body.</span></span>

## <a name="general-queries"></a><span data-ttu-id="b73be-382">Allmänna frågor</span><span class="sxs-lookup"><span data-stu-id="b73be-382">General queries</span></span>
<span data-ttu-id="b73be-383">Allmänna frågor returnera en lista över Service Fabric-enheter på en viss typ av.</span><span class="sxs-lookup"><span data-stu-id="b73be-383">General queries return a list of Service Fabric entities of a specified type.</span></span> <span data-ttu-id="b73be-384">De är tillgängliga via hello API (via hello metoder i **FabricClient.QueryManager**), PowerShell-cmdletar och REST.</span><span class="sxs-lookup"><span data-stu-id="b73be-384">They are exposed through hello API (via hello methods on **FabricClient.QueryManager**), PowerShell cmdlets, and REST.</span></span> <span data-ttu-id="b73be-385">De här frågorna aggregera underfrågor från flera komponenter.</span><span class="sxs-lookup"><span data-stu-id="b73be-385">These queries aggregate subqueries from multiple components.</span></span> <span data-ttu-id="b73be-386">En av dem är hello [hälsoarkivet](service-fabric-health-introduction.md#health-store), som fyller på hello samman hälsotillståndet för varje frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="b73be-386">One of them is hello [health store](service-fabric-health-introduction.md#health-store), which populates hello aggregated health state for each query result.</span></span>  

> [!NOTE]
> <span data-ttu-id="b73be-387">Allmänna frågor returnera hello samman hälsotillståndet för hello entitet och innehåller inte omfattande hälsa data.</span><span class="sxs-lookup"><span data-stu-id="b73be-387">General queries return hello aggregated health state of hello entity and do not contain rich health data.</span></span> <span data-ttu-id="b73be-388">Om en enhet inte är felfri, kan du följa upp med hälsa frågor tooget alla dess hälsa information, inklusive händelser, underordnade hälsotillstånd och feltillstånd utvärderingar.</span><span class="sxs-lookup"><span data-stu-id="b73be-388">If an entity is not healthy, you can follow up with health queries tooget all its health information, including events, child health states, and unhealthy evaluations.</span></span>
>
>

<span data-ttu-id="b73be-389">Om allmänna frågor returnerar ett okänt hälsotillståndet för en entitet, är det möjligt att hello health store inte har slutförts data om hello entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-389">If general queries return an unknown health state for an entity, it's possible that hello health store doesn't have complete data about hello entity.</span></span> <span data-ttu-id="b73be-390">Det är också möjligt att en underfråga toohello hälsoarkivet inte lyckas (t.ex, det uppstod ett kommunikationsfel eller hello health store har begränsats).</span><span class="sxs-lookup"><span data-stu-id="b73be-390">It's also possible that a subquery toohello health store wasn't successful (for example, there was a communication error, or hello health store was throttled).</span></span> <span data-ttu-id="b73be-391">Följa upp med en fråga för hälsa för hello entiteten.</span><span class="sxs-lookup"><span data-stu-id="b73be-391">Follow up with a health query for hello entity.</span></span> <span data-ttu-id="b73be-392">Om hello underfråga påträffade tillfälliga fel, till exempel nätverksproblem, kan den här uppföljning frågan lyckas.</span><span class="sxs-lookup"><span data-stu-id="b73be-392">If hello subquery encountered transient errors, such as network issues, this follow-up query may succeed.</span></span> <span data-ttu-id="b73be-393">Det kan också ge mer information från hello health store om varför exponeras inte hello entitet.</span><span class="sxs-lookup"><span data-stu-id="b73be-393">It may also give you more details from hello health store about why hello entity is not exposed.</span></span>

<span data-ttu-id="b73be-394">Hej frågor som innehåller **HealthState** för entiteter är:</span><span class="sxs-lookup"><span data-stu-id="b73be-394">hello queries that contain **HealthState** for entities are:</span></span>

* <span data-ttu-id="b73be-395">Nodlistan: returnerar hello listan noder i klustret för hello (växlat minne).</span><span class="sxs-lookup"><span data-stu-id="b73be-395">Node list: Returns hello list nodes in hello cluster (paged).</span></span>
  * <span data-ttu-id="b73be-396">API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)</span><span class="sxs-lookup"><span data-stu-id="b73be-396">API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)</span></span>
  * <span data-ttu-id="b73be-397">PowerShell: Get-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="b73be-397">PowerShell: Get-ServiceFabricNode</span></span>
* <span data-ttu-id="b73be-398">Programlista: returnerar hello lista över program i hello kluster (växlat minne).</span><span class="sxs-lookup"><span data-stu-id="b73be-398">Application list: Returns hello list of applications in hello cluster (paged).</span></span>
  * <span data-ttu-id="b73be-399">API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)</span><span class="sxs-lookup"><span data-stu-id="b73be-399">API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)</span></span>
  * <span data-ttu-id="b73be-400">PowerShell: Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="b73be-400">PowerShell: Get-ServiceFabricApplication</span></span>
* <span data-ttu-id="b73be-401">Tjänstlista: returnerar hello lista över tjänster i ett program (växlat minne).</span><span class="sxs-lookup"><span data-stu-id="b73be-401">Service list: Returns hello list of services in an application (paged).</span></span>
  * <span data-ttu-id="b73be-402">API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)</span><span class="sxs-lookup"><span data-stu-id="b73be-402">API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)</span></span>
  * <span data-ttu-id="b73be-403">PowerShell: Get-ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="b73be-403">PowerShell: Get-ServiceFabricService</span></span>
* <span data-ttu-id="b73be-404">Partitionslista: returnerar hello lista över partitioner i en tjänst (växlat minne).</span><span class="sxs-lookup"><span data-stu-id="b73be-404">Partition list: Returns hello list of partitions in a service (paged).</span></span>
  * <span data-ttu-id="b73be-405">API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)</span><span class="sxs-lookup"><span data-stu-id="b73be-405">API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)</span></span>
  * <span data-ttu-id="b73be-406">PowerShell: Get-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="b73be-406">PowerShell: Get-ServiceFabricPartition</span></span>
* <span data-ttu-id="b73be-407">Replikeringslistan: returnerar hello listan med repliker i en partition (växlat minne).</span><span class="sxs-lookup"><span data-stu-id="b73be-407">Replica list: Returns hello list of replicas in a partition (paged).</span></span>
  * <span data-ttu-id="b73be-408">API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)</span><span class="sxs-lookup"><span data-stu-id="b73be-408">API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)</span></span>
  * <span data-ttu-id="b73be-409">PowerShell: Get-ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="b73be-409">PowerShell: Get-ServiceFabricReplica</span></span>
* <span data-ttu-id="b73be-410">Distribuerat programlista: returnerar hello listan med distribuerade program på en nod.</span><span class="sxs-lookup"><span data-stu-id="b73be-410">Deployed application list: Returns hello list of deployed applications on a node.</span></span>
  * <span data-ttu-id="b73be-411">API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)</span><span class="sxs-lookup"><span data-stu-id="b73be-411">API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)</span></span>
  * <span data-ttu-id="b73be-412">PowerShell: Get-ServiceFabricDeployedApplication</span><span class="sxs-lookup"><span data-stu-id="b73be-412">PowerShell: Get-ServiceFabricDeployedApplication</span></span>
* <span data-ttu-id="b73be-413">Distribuera tjänsten paketlistan: returnerar hello lista över servicepaket i ett distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="b73be-413">Deployed service package list: Returns hello list of service packages in a deployed application.</span></span>
  * <span data-ttu-id="b73be-414">API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)</span><span class="sxs-lookup"><span data-stu-id="b73be-414">API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)</span></span>
  * <span data-ttu-id="b73be-415">PowerShell: Get-ServiceFabricDeployedApplication</span><span class="sxs-lookup"><span data-stu-id="b73be-415">PowerShell: Get-ServiceFabricDeployedApplication</span></span>

> [!NOTE]
> <span data-ttu-id="b73be-416">Vissa av hello frågor returnerar växlingsbara resultat.</span><span class="sxs-lookup"><span data-stu-id="b73be-416">Some of hello queries return paged results.</span></span> <span data-ttu-id="b73be-417">hello returnera dessa frågor är en lista som härletts från [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1).</span><span class="sxs-lookup"><span data-stu-id="b73be-417">hello return of these queries is a list derived from [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1).</span></span> <span data-ttu-id="b73be-418">Om hello resultatet inte ryms ett meddelande, returneras endast en sida och en ContinuationToken som spårar där uppräkningen avbröts.</span><span class="sxs-lookup"><span data-stu-id="b73be-418">If hello results do not fit a message, only a page is returned and a ContinuationToken that tracks where enumeration stopped.</span></span> <span data-ttu-id="b73be-419">Fortsätt toocall hello samma fråga och skicka in hello fortsättningstoken från hello tidigare tooget nästa frågeresultaten.</span><span class="sxs-lookup"><span data-stu-id="b73be-419">Continue toocall hello same query and pass in hello continuation token from hello previous query tooget next results.</span></span>
>
>

### <a name="examples"></a><span data-ttu-id="b73be-420">Exempel</span><span class="sxs-lookup"><span data-stu-id="b73be-420">Examples</span></span>
<span data-ttu-id="b73be-421">hello hämtar följande kod hello felaktiga program i hello klustret:</span><span class="sxs-lookup"><span data-stu-id="b73be-421">hello following code gets hello unhealthy applications in hello cluster:</span></span>

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

<span data-ttu-id="b73be-422">hello följande cmdlet hämtar hello programinformation för hello fabric: / WordCount-programmet.</span><span class="sxs-lookup"><span data-stu-id="b73be-422">hello following cmdlet gets hello application details for hello fabric:/WordCount application.</span></span> <span data-ttu-id="b73be-423">Observera att hälsotillståndet är på varningen.</span><span class="sxs-lookup"><span data-stu-id="b73be-423">Notice that health state is at warning.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

<span data-ttu-id="b73be-424">hello hämtar följande cmdlet hello-tjänster med ett hälsotillstånd fel:</span><span class="sxs-lookup"><span data-stu-id="b73be-424">hello following cmdlet gets hello services with a health state of error:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a><span data-ttu-id="b73be-425">Uppgradering av klustret och program</span><span class="sxs-lookup"><span data-stu-id="b73be-425">Cluster and application upgrades</span></span>
<span data-ttu-id="b73be-426">Under en övervakad uppgradering av hello kluster och program kontrollerar Service Fabric hälsa tooensure att allt är felfri.</span><span class="sxs-lookup"><span data-stu-id="b73be-426">During a monitored upgrade of hello cluster and application, Service Fabric checks health tooensure that everything remains healthy.</span></span> <span data-ttu-id="b73be-427">Om en enhet är i feltillstånd som utvärderas med hjälp av konfigurerade hälsoprinciper, gäller hello uppgraderingen uppgraderingen-specifika policys toodetermine hello nästa åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b73be-427">If an entity is unhealthy as evaluated by using configured health policies, hello upgrade applies upgrade-specific policies toodetermine hello next action.</span></span> <span data-ttu-id="b73be-428">hello uppgraderingen kan vara pausas tooallow användarinteraktion (till exempel korrigera felvillkor eller ändra principer) eller den får automatiskt återställa toohello tidigare fungerande versionen.</span><span class="sxs-lookup"><span data-stu-id="b73be-428">hello upgrade may be paused tooallow user interaction (such as fixing error conditions or changing policies), or it may automatically roll back toohello previous good version.</span></span>

<span data-ttu-id="b73be-429">Under en *klustret* uppgraderar du hello uppgraderingsstatus för klustret.</span><span class="sxs-lookup"><span data-stu-id="b73be-429">During a *cluster* upgrade, you can get hello cluster upgrade status.</span></span> <span data-ttu-id="b73be-430">hello uppgraderingsstatus innehåller felaktiga utvärderingar, vilka punkt toowhat är i feltillstånd i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="b73be-430">hello upgrade status includes unhealthy evaluations, which point toowhat is unhealthy in hello cluster.</span></span> <span data-ttu-id="b73be-431">Om uppgraderingen hello återställs på grund av problem med toohealth ihåg hello uppgraderingsstatus hello senaste ohälsosamt orsaker.</span><span class="sxs-lookup"><span data-stu-id="b73be-431">If hello upgrade is rolled back due toohealth issues, hello upgrade status remembers hello last unhealthy reasons.</span></span> <span data-ttu-id="b73be-432">Den här informationen kan hjälpa administratörer att undersöka vad som gick fel när hello uppgraderingen återställde eller stoppats.</span><span class="sxs-lookup"><span data-stu-id="b73be-432">This information can help administrators investigate what went wrong after hello upgrade rolled back or stopped.</span></span>

<span data-ttu-id="b73be-433">På liknande sätt under en *programmet* uppgraderar alla feltillstånd utvärderingar finns i hello uppgraderingsstatus för programmet.</span><span class="sxs-lookup"><span data-stu-id="b73be-433">Similarly, during an *application* upgrade, any unhealthy evaluations are contained in hello application upgrade status.</span></span>

<span data-ttu-id="b73be-434">hello följande visar hello programmet uppgraderingsstatus för ändrade fabric: / WordCount-programmet.</span><span class="sxs-lookup"><span data-stu-id="b73be-434">hello following shows hello application upgrade status for a modified fabric:/WordCount application.</span></span> <span data-ttu-id="b73be-435">En watchdog rapporterade ett fel på en av dess repliker.</span><span class="sxs-lookup"><span data-stu-id="b73be-435">A watchdog reported an error on one of its replicas.</span></span> <span data-ttu-id="b73be-436">hello uppgraderingen rullande eftersom hello hälsokontroller inte uppfylls.</span><span class="sxs-lookup"><span data-stu-id="b73be-436">hello upgrade is rolling back because hello health checks are not respected.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

<span data-ttu-id="b73be-437">Läs mer om hello [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="b73be-437">Read more about hello [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="use-health-evaluations-tootroubleshoot"></a><span data-ttu-id="b73be-438">Använd hälsa utvärderingar tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="b73be-438">Use health evaluations tootroubleshoot</span></span>
<span data-ttu-id="b73be-439">När det finns ett problem med hello kluster eller ett program, ser hello kluster eller ett program hälsa toopinpoint fel.</span><span class="sxs-lookup"><span data-stu-id="b73be-439">Whenever there is an issue with hello cluster or an application, look at hello cluster or application health toopinpoint what is wrong.</span></span> <span data-ttu-id="b73be-440">hello ohälsosamt utvärderingar innehåller information om vilka utlösta hello aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-440">hello unhealthy evaluations provide details about what triggered hello current unhealthy state.</span></span> <span data-ttu-id="b73be-441">Om du behöver, kan du detaljnivån i feltillstånd underordnade entiteter tooidentify hello orsaken.</span><span class="sxs-lookup"><span data-stu-id="b73be-441">If you need to, you can drill down into unhealthy child entities tooidentify hello root cause.</span></span>

<span data-ttu-id="b73be-442">Tänk dig ett program ohälsosamt eftersom det inte finns en felrapport på en av dess repliker.</span><span class="sxs-lookup"><span data-stu-id="b73be-442">For example, consider an application unhealthy because there is an error report on one of its replicas.</span></span> <span data-ttu-id="b73be-443">hello visar följande Powershell-cmdlet hello ohälsosamt utvärderingar:</span><span class="sxs-lookup"><span data-stu-id="b73be-443">hello following Powershell cmdlet shows hello unhealthy evaluations:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.
                                  
                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.
                                  
                                            Error event: SourceId='MyWatchdog', Property='Memory'.
                                  
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

<span data-ttu-id="b73be-444">Du kan titta på hello replik tooget mer information:</span><span class="sxs-lookup"><span data-stu-id="b73be-444">You can look at hello replica tooget more information:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> <span data-ttu-id="b73be-445">hello utvärderas ohälsosamt utvärderingar visar hello första skälet hello entiteten är toocurrent hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="b73be-445">hello unhealthy evaluations show hello first reason hello entity is evaluated toocurrent health state.</span></span> <span data-ttu-id="b73be-446">Det kan finnas flera andra händelser som utlöser detta tillstånd, men de är inte återspeglas i hello utvärderingar.</span><span class="sxs-lookup"><span data-stu-id="b73be-446">There may be multiple other events that trigger this state, but they are not be reflected in hello evaluations.</span></span> <span data-ttu-id="b73be-447">tooget mer information, ökad detaljnivå i hello hälsa entiteter toofigure ut alla hello ohälsosamt rapporter i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="b73be-447">tooget more information, drill down into hello health entities toofigure out all hello unhealthy reports in hello cluster.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="b73be-448">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b73be-448">Next steps</span></span>
[<span data-ttu-id="b73be-449">Använd system hälsa rapporter tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="b73be-449">Use system health reports tootroubleshoot</span></span>](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[<span data-ttu-id="b73be-450">Lägg till anpassade hälsorapporter för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b73be-450">Add custom Service Fabric health reports</span></span>](service-fabric-report-health.md)

[<span data-ttu-id="b73be-451">Hur tooreport och kontrollera-tjänsten för hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="b73be-451">How tooreport and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="b73be-452">Övervaka och diagnostisera tjänster lokalt</span><span class="sxs-lookup"><span data-stu-id="b73be-452">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="b73be-453">Uppgradering av Service Fabric-programmet</span><span class="sxs-lookup"><span data-stu-id="b73be-453">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)
