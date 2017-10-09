---
title: "aaaAzure Service Fabric resurs styrning för behållare och tjänster | Microsoft Docs"
description: "Azure Service Fabric kan toospecify gränserna för tjänster som körs inom eller utanför behållare."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 34e368211d98ff6b5b294c9c8b3af5ca30eeb20c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-governance"></a><span data-ttu-id="188e7-103">Resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="188e7-103">Resource governance</span></span> 

<span data-ttu-id="188e7-104">När hello flera tjänster körs på samma nod eller kluster, är det möjligt att en tjänst kan förbruka mer resurser starving andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="188e7-104">When running multiple services on hello same node or cluster, it is possible that one service might consume more resources starving other services.</span></span> <span data-ttu-id="188e7-105">Det här problemet är refererad tooas hello störningar granne problem.</span><span class="sxs-lookup"><span data-stu-id="188e7-105">This problem is referred tooas hello noisy-neighbor problem.</span></span> <span data-ttu-id="188e7-106">Service Fabric kan hello developer toospecify reservationer och gränser för tjänsten tooguarantee resurser och också begränsa dess Resursanvändning.</span><span class="sxs-lookup"><span data-stu-id="188e7-106">Service Fabric allows hello developer toospecify reservations and limits per service tooguarantee resources and also limit its resource usage.</span></span> 

## <a name="resource-governance-metrics"></a><span data-ttu-id="188e7-107">Resursen styrning mått</span><span class="sxs-lookup"><span data-stu-id="188e7-107">Resource governance metrics</span></span> 

<span data-ttu-id="188e7-108">Resurs-styrning stöds i Service Fabric per [tjänstpaket](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="188e7-108">Resource governance is supported in Service Fabric per [Service Package](service-fabric-application-model.md).</span></span> <span data-ttu-id="188e7-109">hello-resurser som är tilldelade tooService paketet kan ytterligare delas mellan kod paket.</span><span class="sxs-lookup"><span data-stu-id="188e7-109">hello resources that are assigned tooService Package can be further divided between code packages.</span></span> <span data-ttu-id="188e7-110">gränserna för hello anges också vara hello reservation av hello resurser.</span><span class="sxs-lookup"><span data-stu-id="188e7-110">hello resource limits specified also mean hello reservation of hello resources.</span></span> <span data-ttu-id="188e7-111">Service Fabric stöder anger CPU och minne per servicepaket, med två inbyggda [mått](service-fabric-cluster-resource-manager-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="188e7-111">Service Fabric supports specifying CPU and Memory per service package, using two built-in [metrics](service-fabric-cluster-resource-manager-metrics.md):</span></span>

* <span data-ttu-id="188e7-112">CPU (måttnamnet `ServiceFabric:/_CpuCores`): en kärna är en logisk kärna som är tillgängligt på värddatorn för hello och alla kärnor på alla noder viktas hello samma.</span><span class="sxs-lookup"><span data-stu-id="188e7-112">CPU (metric name `ServiceFabric:/_CpuCores`): A core is a logical core that is available on hello host machine, and all cores across all nodes are weighted hello same.</span></span>
* <span data-ttu-id="188e7-113">Minne (måttnamnet `ServiceFabric:/_MemoryInMB`): uttrycks minne i megabyte och mappas toophysical minne som är tillgängligt på hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="188e7-113">Memory (metric name `ServiceFabric:/_MemoryInMB`): Memory is expressed in megabytes, and it maps toophysical memory that is available on hello machine.</span></span>

<span data-ttu-id="188e7-114">Endast tillhandahålls mjuk procentuell garantier - hello runtime avvisar öppnandet av ny tjänst paket tillgängliga resurser har överskridits.</span><span class="sxs-lookup"><span data-stu-id="188e7-114">Only soft reservation guarantees are provided - hello runtime rejects opening of new service packages available resources are exceeded.</span></span> <span data-ttu-id="188e7-115">Om en annan körbar fil eller behållare är placerad hello nod, som dock bryta hello ursprungliga reservation garantier.</span><span class="sxs-lookup"><span data-stu-id="188e7-115">However, if another executable or container is placed on hello node, that may violate hello original reservation guarantees.</span></span>

<span data-ttu-id="188e7-116">Dessa två mått hello [klustret Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) spårar totala klustret kapacitet, hello belastningen på varje nod i klustret hello och återstående resurser i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="188e7-116">For these two metrics, hello [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) tracks total cluster capacity, hello load on each node in hello cluster, and, remaining resources in hello cluster.</span></span> <span data-ttu-id="188e7-117">Dessa två mått har motsvarande tooany eller andra anpassade mått och alla befintliga funktioner kan användas med dem:</span><span class="sxs-lookup"><span data-stu-id="188e7-117">These two metrics are equivalent tooany other user or custom metric and all existing features can be used with them:</span></span>
* <span data-ttu-id="188e7-118">Klustret kan vara [belastningsutjämnade](service-fabric-cluster-resource-manager-balancing.md) enligt toothese två mått (standardinställning).</span><span class="sxs-lookup"><span data-stu-id="188e7-118">Cluster can be [balanced](service-fabric-cluster-resource-manager-balancing.md) according toothese two metrics (default behavior).</span></span>
* <span data-ttu-id="188e7-119">Klustret kan vara [defragmenteras](service-fabric-cluster-resource-manager-defragmentation-metrics.md) enligt toothese två mått.</span><span class="sxs-lookup"><span data-stu-id="188e7-119">Cluster can be [defragmented](service-fabric-cluster-resource-manager-defragmentation-metrics.md) according toothese two metrics.</span></span>
* <span data-ttu-id="188e7-120">När [som beskriver ett kluster](service-fabric-cluster-resource-manager-cluster-description.md), buffrat kapacitet kan ställas in för dessa två mått.</span><span class="sxs-lookup"><span data-stu-id="188e7-120">When [describing a cluster](service-fabric-cluster-resource-manager-cluster-description.md), buffered capacity can be set for these two metrics.</span></span>

<span data-ttu-id="188e7-121">[Dynamisk reporting](service-fabric-cluster-resource-manager-metrics.md) stöds inte för de här måtten och läser in för dessa mått har definierats vid skapandet.</span><span class="sxs-lookup"><span data-stu-id="188e7-121">[Dynamic load reporting](service-fabric-cluster-resource-manager-metrics.md) is not supported for these metrics, and loads for these metrics are defined at creation time.</span></span>

## <a name="cluster-set-up-for-enabling-resource-governance"></a><span data-ttu-id="188e7-122">Set-kluster för att aktivera resursen styrning</span><span class="sxs-lookup"><span data-stu-id="188e7-122">Cluster set up for enabling resource governance</span></span>

<span data-ttu-id="188e7-123">Kapacitet ska vara definierat manuellt i varje nodtyp i hello kluster på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="188e7-123">Capacity should be defined manually in each node type in hello cluster as follows:</span></span>

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
<span data-ttu-id="188e7-124">Resurs-styrning tillåts endast på användare, tjänster och inte på alla systemtjänster.</span><span class="sxs-lookup"><span data-stu-id="188e7-124">Resource governance is allowed only on user services and not on any system services.</span></span> <span data-ttu-id="188e7-125">När du anger kapacitet, vissa kärnor och minne måste vara vänster oallokerat för systemtjänster.</span><span class="sxs-lookup"><span data-stu-id="188e7-125">When specifying capacity, some cores and memory must be left unallocated for system services.</span></span> <span data-ttu-id="188e7-126">För optimala prestanda bör också hello följande inställning aktiveras i klustermanifestet hello:</span><span class="sxs-lookup"><span data-stu-id="188e7-126">For optimal performance, hello following setting should also be turned on in hello cluster manifest:</span></span> 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a><span data-ttu-id="188e7-127">Ange resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="188e7-127">Specifying resource governance</span></span> 

<span data-ttu-id="188e7-128">Gränserna för styrning har angetts i applikationsmanifestet hello (ServiceManifestImport avsnittet) som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="188e7-128">Resource governance limits are specified in hello application manifest (ServiceManifestImport section) as shown in hello following example:</span></span>

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has hello number of CPU cores defined, but doesn't have hello MemoryInMB defined.
  In this case, Service Fabric will sum hello limits on code packages and uses hello sum as 
  hello overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```
  
<span data-ttu-id="188e7-129">I det här exemplet hämtar servicepaket ServicePackageA en kärna på hello noder där den är placerad.</span><span class="sxs-lookup"><span data-stu-id="188e7-129">In this example, service package ServicePackageA gets one core on hello nodes where it is placed.</span></span> <span data-ttu-id="188e7-130">Det här service-paketet innehåller två kod-paket (CodeA1 och CodeA2) och både ange hello `CpuShares` parameter.</span><span class="sxs-lookup"><span data-stu-id="188e7-130">This service package contains two code packages (CodeA1 and CodeA2), and both specify hello `CpuShares` parameter.</span></span> <span data-ttu-id="188e7-131">hello andelen CpuShares 512:256 dividerar hello core över hello två kod paket.</span><span class="sxs-lookup"><span data-stu-id="188e7-131">hello proportion of CpuShares 512:256  divides hello core across hello two code packages.</span></span> <span data-ttu-id="188e7-132">Därför i det här exemplet CodeA1 hämtar en kärna väger CodeA2 hämtar en tredjedel av en kärna (och en mjuk garanti reservation av hello samma).</span><span class="sxs-lookup"><span data-stu-id="188e7-132">Thus, in this example, CodeA1 gets two-thirds of a core, and  CodeA2 gets one-third of a core (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="188e7-133">Om när CpuShares inte har angetts för koden paket, delar hello kärnor balanseras mellan Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="188e7-133">In case when CpuShares are not specified for code packages, Service Fabric divides hello cores equally among them.</span></span>

<span data-ttu-id="188e7-134">Minnesgränserna är absolut, så att båda paketen koden är begränsad too1024 MB minne (och en mjuk garanti reservation av hello samma).</span><span class="sxs-lookup"><span data-stu-id="188e7-134">Memory limits are absolute, so both code packages are limited too1024 MB of memory (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="188e7-135">Koden paket (behållare eller processer) är inte kan tooallocate mer minne än den här gränsen och försök toodo så resulterar i ett undantag i minnet är slut.</span><span class="sxs-lookup"><span data-stu-id="188e7-135">Code packages (containers or processes) are not able tooallocate more memory than this limit, and attempting toodo so results in an out-of-memory exception.</span></span> <span data-ttu-id="188e7-136">Resursen gränsen tvingande toowork har alla kod paket i ett tjänstepaket minnesgränserna som angetts.</span><span class="sxs-lookup"><span data-stu-id="188e7-136">For resource limit enforcement toowork, all code packages within a service package should have memory limits specified.</span></span>


## <a name="next-steps"></a><span data-ttu-id="188e7-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="188e7-137">Next steps</span></span>
* <span data-ttu-id="188e7-138">toolearn mer om klustret Resource Manager kan läsa det här [artikel](service-fabric-cluster-resource-manager-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="188e7-138">toolearn more about Cluster Resource Manager, read this [article](service-fabric-cluster-resource-manager-introduction.md).</span></span>
* <span data-ttu-id="188e7-139">Läs toolearn mer om programmodell, servicepaket, kod paket och hur repliker mappa toothem [artikel](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="188e7-139">toolearn more about application model, service packages, code packages and how replicas map toothem read this [article](service-fabric-application-model.md).</span></span>
