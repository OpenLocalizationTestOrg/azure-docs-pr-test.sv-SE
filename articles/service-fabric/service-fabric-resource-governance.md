---
title: "Azure Service Fabric resurs styrning för behållare och tjänster | Microsoft Docs"
description: "Azure Service Fabric kan du ange gränserna för tjänster som körs inom eller utanför behållare."
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
ms.openlocfilehash: 88d44953ad83f9e7401fd087a39842e4a3790124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="resource-governance"></a><span data-ttu-id="9cadc-103">Resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="9cadc-103">Resource governance</span></span> 

<span data-ttu-id="9cadc-104">När du kör flera tjänster på samma nod eller i klustret, är det möjligt att en tjänst kan förbruka mer resurser starving andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="9cadc-104">When running multiple services on the same node or cluster, it is possible that one service might consume more resources starving other services.</span></span> <span data-ttu-id="9cadc-105">Det här problemet kallas störningar granne problemet.</span><span class="sxs-lookup"><span data-stu-id="9cadc-105">This problem is referred to as the noisy-neighbor problem.</span></span> <span data-ttu-id="9cadc-106">Service Fabric kan utvecklare ange reservationer och gränser för tjänsten för att garantera resurser och också begränsa dess Resursanvändning.</span><span class="sxs-lookup"><span data-stu-id="9cadc-106">Service Fabric allows the developer to specify reservations and limits per service to guarantee resources and also limit its resource usage.</span></span> 

## <a name="resource-governance-metrics"></a><span data-ttu-id="9cadc-107">Resursen styrning mått</span><span class="sxs-lookup"><span data-stu-id="9cadc-107">Resource governance metrics</span></span> 

<span data-ttu-id="9cadc-108">Resurs-styrning stöds i Service Fabric per [tjänstpaket](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="9cadc-108">Resource governance is supported in Service Fabric per [Service Package](service-fabric-application-model.md).</span></span> <span data-ttu-id="9cadc-109">De resurser som har tilldelats tjänstpaket kan ytterligare delas mellan kod paket.</span><span class="sxs-lookup"><span data-stu-id="9cadc-109">The resources that are assigned to Service Package can be further divided between code packages.</span></span> <span data-ttu-id="9cadc-110">De angivna gränserna för resursen även innebära reservation av resurser.</span><span class="sxs-lookup"><span data-stu-id="9cadc-110">The resource limits specified also mean the reservation of the resources.</span></span> <span data-ttu-id="9cadc-111">Service Fabric stöder anger CPU och minne per servicepaket, med två inbyggda [mått](service-fabric-cluster-resource-manager-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="9cadc-111">Service Fabric supports specifying CPU and Memory per service package, using two built-in [metrics](service-fabric-cluster-resource-manager-metrics.md):</span></span>

* <span data-ttu-id="9cadc-112">CPU (måttnamnet `ServiceFabric:/_CpuCores`): en kärna är en logisk kärna som är tillgängligt på värddatorn och alla kärnor på alla noder viktas samma.</span><span class="sxs-lookup"><span data-stu-id="9cadc-112">CPU (metric name `ServiceFabric:/_CpuCores`): A core is a logical core that is available on the host machine, and all cores across all nodes are weighted the same.</span></span>
* <span data-ttu-id="9cadc-113">Minne (måttnamnet `ServiceFabric:/_MemoryInMB`): uttrycks minne i megabyte och mappas till det fysiska minne som är tillgängligt på datorn.</span><span class="sxs-lookup"><span data-stu-id="9cadc-113">Memory (metric name `ServiceFabric:/_MemoryInMB`): Memory is expressed in megabytes, and it maps to physical memory that is available on the machine.</span></span>

<span data-ttu-id="9cadc-114">Endast tillhandahålls mjuk procentuell garantier - körningsmiljön avvisar öppnandet av nya servicepaket tillgängliga resurser har överskridits.</span><span class="sxs-lookup"><span data-stu-id="9cadc-114">Only soft reservation guarantees are provided - the runtime rejects opening of new service packages available resources are exceeded.</span></span> <span data-ttu-id="9cadc-115">Om en annan körbar fil eller behållaren placeras på noden, som dock bryta ursprungliga reservation garantier.</span><span class="sxs-lookup"><span data-stu-id="9cadc-115">However, if another executable or container is placed on the node, that may violate the original reservation guarantees.</span></span>

<span data-ttu-id="9cadc-116">För dessa två mått i [klustret Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) spårar totala klustret kapacitet, belastningen på varje nod i klustret, och återstående resurser i klustret.</span><span class="sxs-lookup"><span data-stu-id="9cadc-116">For these two metrics, the [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) tracks total cluster capacity, the load on each node in the cluster, and, remaining resources in the cluster.</span></span> <span data-ttu-id="9cadc-117">Dessa två mått är likvärdiga med andra användare eller anpassade mått och alla befintliga funktioner kan användas med dem:</span><span class="sxs-lookup"><span data-stu-id="9cadc-117">These two metrics are equivalent to any other user or custom metric and all existing features can be used with them:</span></span>
* <span data-ttu-id="9cadc-118">Klustret kan vara [belastningsutjämnade](service-fabric-cluster-resource-manager-balancing.md) enligt dessa två mått (standardinställning).</span><span class="sxs-lookup"><span data-stu-id="9cadc-118">Cluster can be [balanced](service-fabric-cluster-resource-manager-balancing.md) according to these two metrics (default behavior).</span></span>
* <span data-ttu-id="9cadc-119">Klustret kan vara [defragmenteras](service-fabric-cluster-resource-manager-defragmentation-metrics.md) enligt dessa två mått.</span><span class="sxs-lookup"><span data-stu-id="9cadc-119">Cluster can be [defragmented](service-fabric-cluster-resource-manager-defragmentation-metrics.md) according to these two metrics.</span></span>
* <span data-ttu-id="9cadc-120">När [som beskriver ett kluster](service-fabric-cluster-resource-manager-cluster-description.md), buffrat kapacitet kan ställas in för dessa två mått.</span><span class="sxs-lookup"><span data-stu-id="9cadc-120">When [describing a cluster](service-fabric-cluster-resource-manager-cluster-description.md), buffered capacity can be set for these two metrics.</span></span>

<span data-ttu-id="9cadc-121">[Dynamisk reporting](service-fabric-cluster-resource-manager-metrics.md) stöds inte för de här måtten och läser in för dessa mått har definierats vid skapandet.</span><span class="sxs-lookup"><span data-stu-id="9cadc-121">[Dynamic load reporting](service-fabric-cluster-resource-manager-metrics.md) is not supported for these metrics, and loads for these metrics are defined at creation time.</span></span>

## <a name="cluster-set-up-for-enabling-resource-governance"></a><span data-ttu-id="9cadc-122">Set-kluster för att aktivera resursen styrning</span><span class="sxs-lookup"><span data-stu-id="9cadc-122">Cluster set up for enabling resource governance</span></span>

<span data-ttu-id="9cadc-123">Kapacitet ska vara definierat manuellt i varje nodtyp i klustret på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9cadc-123">Capacity should be defined manually in each node type in the cluster as follows:</span></span>

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
<span data-ttu-id="9cadc-124">Resurs-styrning tillåts endast på användare, tjänster och inte på alla systemtjänster.</span><span class="sxs-lookup"><span data-stu-id="9cadc-124">Resource governance is allowed only on user services and not on any system services.</span></span> <span data-ttu-id="9cadc-125">När du anger kapacitet, vissa kärnor och minne måste vara vänster oallokerat för systemtjänster.</span><span class="sxs-lookup"><span data-stu-id="9cadc-125">When specifying capacity, some cores and memory must be left unallocated for system services.</span></span> <span data-ttu-id="9cadc-126">För optimala prestanda bör också följande inställning aktiveras i klustermanifestet:</span><span class="sxs-lookup"><span data-stu-id="9cadc-126">For optimal performance, the following setting should also be turned on in the cluster manifest:</span></span> 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a><span data-ttu-id="9cadc-127">Ange resurs-styrning</span><span class="sxs-lookup"><span data-stu-id="9cadc-127">Specifying resource governance</span></span> 

<span data-ttu-id="9cadc-128">Gränserna för styrning har angetts i applikationsmanifestet (ServiceManifestImport avsnittet) som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="9cadc-128">Resource governance limits are specified in the application manifest (ServiceManifestImport section) as shown in the following example:</span></span>

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric will sum the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
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
  
<span data-ttu-id="9cadc-129">I det här exemplet hämtar servicepaket ServicePackageA en core på noderna där den är placerad.</span><span class="sxs-lookup"><span data-stu-id="9cadc-129">In this example, service package ServicePackageA gets one core on the nodes where it is placed.</span></span> <span data-ttu-id="9cadc-130">Det här service-paketet innehåller två kod-paket (CodeA1 och CodeA2) och både ange den `CpuShares` parameter.</span><span class="sxs-lookup"><span data-stu-id="9cadc-130">This service package contains two code packages (CodeA1 and CodeA2), and both specify the `CpuShares` parameter.</span></span> <span data-ttu-id="9cadc-131">Andelen CpuShares 512:256 delar upp grundläggande mellan de två kod paket.</span><span class="sxs-lookup"><span data-stu-id="9cadc-131">The proportion of CpuShares 512:256  divides the core across the two code packages.</span></span> <span data-ttu-id="9cadc-132">Därför i det här exemplet CodeA1 hämtar en kärna väger och CodeA2 hämtar en tredjedel av en kärna (och en mjuk garanti reservation av samma).</span><span class="sxs-lookup"><span data-stu-id="9cadc-132">Thus, in this example, CodeA1 gets two-thirds of a core, and  CodeA2 gets one-third of a core (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="9cadc-133">Om när CpuShares inte har angetts för koden paket, delar kärnor balanseras mellan Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9cadc-133">In case when CpuShares are not specified for code packages, Service Fabric divides the cores equally among them.</span></span>

<span data-ttu-id="9cadc-134">Minnesgränserna är absolut, så att båda paketen koden är begränsade till 1 024 MB minne (och en mjuk garanti reservation av samma).</span><span class="sxs-lookup"><span data-stu-id="9cadc-134">Memory limits are absolute, so both code packages are limited to 1024 MB of memory (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="9cadc-135">Kodpaket (behållare eller processer) kan inte tilldela mer minne än den här gränsen, och försök att göra detta leder till undantag utanför minnet.</span><span class="sxs-lookup"><span data-stu-id="9cadc-135">Code packages (containers or processes) are not able to allocate more memory than this limit, and attempting to do so results in an out-of-memory exception.</span></span> <span data-ttu-id="9cadc-136">För att tvingande resursbegränsning ska fungera bör minnesbegränsningar ha angetts för alla kodpaket inom ett tjänstpaket.</span><span class="sxs-lookup"><span data-stu-id="9cadc-136">For resource limit enforcement to work, all code packages within a service package should have memory limits specified.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9cadc-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9cadc-137">Next steps</span></span>
* <span data-ttu-id="9cadc-138">Om du vill veta mer om klustret Resource Manager kan läsa det här [artikel](service-fabric-cluster-resource-manager-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9cadc-138">To learn more about Cluster Resource Manager, read this [article](service-fabric-cluster-resource-manager-introduction.md).</span></span>
* <span data-ttu-id="9cadc-139">Mer information om programmodell servicepaket, kod paket och hur repliker mappas till dem. läsa detta [artikel](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="9cadc-139">To learn more about application model, service packages, code packages and how replicas map to them read this [article](service-fabric-application-model.md).</span></span>
