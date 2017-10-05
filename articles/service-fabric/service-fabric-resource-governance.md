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
# <a name="resource-governance"></a>Resurs-styrning 

När du kör flera tjänster på samma nod eller i klustret, är det möjligt att en tjänst kan förbruka mer resurser starving andra tjänster. Det här problemet kallas störningar granne problemet. Service Fabric kan utvecklare ange reservationer och gränser för tjänsten för att garantera resurser och också begränsa dess Resursanvändning. 

## <a name="resource-governance-metrics"></a>Resursen styrning mått 

Resurs-styrning stöds i Service Fabric per [tjänstpaket](service-fabric-application-model.md). De resurser som har tilldelats tjänstpaket kan ytterligare delas mellan kod paket. De angivna gränserna för resursen även innebära reservation av resurser. Service Fabric stöder anger CPU och minne per servicepaket, med två inbyggda [mått](service-fabric-cluster-resource-manager-metrics.md):

* CPU (måttnamnet `ServiceFabric:/_CpuCores`): en kärna är en logisk kärna som är tillgängligt på värddatorn och alla kärnor på alla noder viktas samma.
* Minne (måttnamnet `ServiceFabric:/_MemoryInMB`): uttrycks minne i megabyte och mappas till det fysiska minne som är tillgängligt på datorn.

Endast tillhandahålls mjuk procentuell garantier - körningsmiljön avvisar öppnandet av nya servicepaket tillgängliga resurser har överskridits. Om en annan körbar fil eller behållaren placeras på noden, som dock bryta ursprungliga reservation garantier.

För dessa två mått i [klustret Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) spårar totala klustret kapacitet, belastningen på varje nod i klustret, och återstående resurser i klustret. Dessa två mått är likvärdiga med andra användare eller anpassade mått och alla befintliga funktioner kan användas med dem:
* Klustret kan vara [belastningsutjämnade](service-fabric-cluster-resource-manager-balancing.md) enligt dessa två mått (standardinställning).
* Klustret kan vara [defragmenteras](service-fabric-cluster-resource-manager-defragmentation-metrics.md) enligt dessa två mått.
* När [som beskriver ett kluster](service-fabric-cluster-resource-manager-cluster-description.md), buffrat kapacitet kan ställas in för dessa två mått.

[Dynamisk reporting](service-fabric-cluster-resource-manager-metrics.md) stöds inte för de här måtten och läser in för dessa mått har definierats vid skapandet.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>Set-kluster för att aktivera resursen styrning

Kapacitet ska vara definierat manuellt i varje nodtyp i klustret på följande sätt:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
Resurs-styrning tillåts endast på användare, tjänster och inte på alla systemtjänster. När du anger kapacitet, vissa kärnor och minne måste vara vänster oallokerat för systemtjänster. För optimala prestanda bör också följande inställning aktiveras i klustermanifestet: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a>Ange resurs-styrning 

Gränserna för styrning har angetts i applikationsmanifestet (ServiceManifestImport avsnittet) som visas i följande exempel:

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
  
I det här exemplet hämtar servicepaket ServicePackageA en core på noderna där den är placerad. Det här service-paketet innehåller två kod-paket (CodeA1 och CodeA2) och både ange den `CpuShares` parameter. Andelen CpuShares 512:256 delar upp grundläggande mellan de två kod paket. Därför i det här exemplet CodeA1 hämtar en kärna väger och CodeA2 hämtar en tredjedel av en kärna (och en mjuk garanti reservation av samma). Om när CpuShares inte har angetts för koden paket, delar kärnor balanseras mellan Service Fabric.

Minnesgränserna är absolut, så att båda paketen koden är begränsade till 1 024 MB minne (och en mjuk garanti reservation av samma). Kodpaket (behållare eller processer) kan inte tilldela mer minne än den här gränsen, och försök att göra detta leder till undantag utanför minnet. För att tvingande resursbegränsning ska fungera bör minnesbegränsningar ha angetts för alla kodpaket inom ett tjänstpaket.


## <a name="next-steps"></a>Nästa steg
* Om du vill veta mer om klustret Resource Manager kan läsa det här [artikel](service-fabric-cluster-resource-manager-introduction.md).
* Mer information om programmodell servicepaket, kod paket och hur repliker mappas till dem. läsa detta [artikel](service-fabric-application-model.md).
