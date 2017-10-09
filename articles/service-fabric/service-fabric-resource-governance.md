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
# <a name="resource-governance"></a>Resurs-styrning 

När hello flera tjänster körs på samma nod eller kluster, är det möjligt att en tjänst kan förbruka mer resurser starving andra tjänster. Det här problemet är refererad tooas hello störningar granne problem. Service Fabric kan hello developer toospecify reservationer och gränser för tjänsten tooguarantee resurser och också begränsa dess Resursanvändning. 

## <a name="resource-governance-metrics"></a>Resursen styrning mått 

Resurs-styrning stöds i Service Fabric per [tjänstpaket](service-fabric-application-model.md). hello-resurser som är tilldelade tooService paketet kan ytterligare delas mellan kod paket. gränserna för hello anges också vara hello reservation av hello resurser. Service Fabric stöder anger CPU och minne per servicepaket, med två inbyggda [mått](service-fabric-cluster-resource-manager-metrics.md):

* CPU (måttnamnet `ServiceFabric:/_CpuCores`): en kärna är en logisk kärna som är tillgängligt på värddatorn för hello och alla kärnor på alla noder viktas hello samma.
* Minne (måttnamnet `ServiceFabric:/_MemoryInMB`): uttrycks minne i megabyte och mappas toophysical minne som är tillgängligt på hello-datorn.

Endast tillhandahålls mjuk procentuell garantier - hello runtime avvisar öppnandet av ny tjänst paket tillgängliga resurser har överskridits. Om en annan körbar fil eller behållare är placerad hello nod, som dock bryta hello ursprungliga reservation garantier.

Dessa två mått hello [klustret Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) spårar totala klustret kapacitet, hello belastningen på varje nod i klustret hello och återstående resurser i hello kluster. Dessa två mått har motsvarande tooany eller andra anpassade mått och alla befintliga funktioner kan användas med dem:
* Klustret kan vara [belastningsutjämnade](service-fabric-cluster-resource-manager-balancing.md) enligt toothese två mått (standardinställning).
* Klustret kan vara [defragmenteras](service-fabric-cluster-resource-manager-defragmentation-metrics.md) enligt toothese två mått.
* När [som beskriver ett kluster](service-fabric-cluster-resource-manager-cluster-description.md), buffrat kapacitet kan ställas in för dessa två mått.

[Dynamisk reporting](service-fabric-cluster-resource-manager-metrics.md) stöds inte för de här måtten och läser in för dessa mått har definierats vid skapandet.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>Set-kluster för att aktivera resursen styrning

Kapacitet ska vara definierat manuellt i varje nodtyp i hello kluster på följande sätt:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
Resurs-styrning tillåts endast på användare, tjänster och inte på alla systemtjänster. När du anger kapacitet, vissa kärnor och minne måste vara vänster oallokerat för systemtjänster. För optimala prestanda bör också hello följande inställning aktiveras i klustermanifestet hello: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a>Ange resurs-styrning 

Gränserna för styrning har angetts i applikationsmanifestet hello (ServiceManifestImport avsnittet) som visas i följande exempel hello:

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
  
I det här exemplet hämtar servicepaket ServicePackageA en kärna på hello noder där den är placerad. Det här service-paketet innehåller två kod-paket (CodeA1 och CodeA2) och både ange hello `CpuShares` parameter. hello andelen CpuShares 512:256 dividerar hello core över hello två kod paket. Därför i det här exemplet CodeA1 hämtar en kärna väger CodeA2 hämtar en tredjedel av en kärna (och en mjuk garanti reservation av hello samma). Om när CpuShares inte har angetts för koden paket, delar hello kärnor balanseras mellan Service Fabric.

Minnesgränserna är absolut, så att båda paketen koden är begränsad too1024 MB minne (och en mjuk garanti reservation av hello samma). Koden paket (behållare eller processer) är inte kan tooallocate mer minne än den här gränsen och försök toodo så resulterar i ett undantag i minnet är slut. Resursen gränsen tvingande toowork har alla kod paket i ett tjänstepaket minnesgränserna som angetts.


## <a name="next-steps"></a>Nästa steg
* toolearn mer om klustret Resource Manager kan läsa det här [artikel](service-fabric-cluster-resource-manager-introduction.md).
* Läs toolearn mer om programmodell, servicepaket, kod paket och hur repliker mappa toothem [artikel](service-fabric-application-model.md).
