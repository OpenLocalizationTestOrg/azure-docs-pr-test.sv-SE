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
# <a name="service-movement-cost"></a>Tjänsten förflyttningskostnad
En faktor som hello resurshanteraren för Service Fabric-klustret anser vid toodetermine vilka ändringar toomake tooa kluster är hello kostnaden för dessa ändringar. hello begreppet ”kostnad” säljs ut mot hur mycket hello-klustret kan förbättras. Kostnaden är inberäknade vid flytt av tjänster för belastningsutjämning, defragmentering och andra krav. hello målet är toomeet hello krav i hello minst störande och dyr sätt. 

Flytta services kostnader CPU-tid och nätverksbandbredd på minst. För tillståndskänsliga tjänster kräver kopiera hello tillståndet för de tjänster som förbrukar mer minne och diskutrymme. Minimera hello kostnaden för lösningar som hello Azure Service Fabric klustret Resource Manager har säkerställer att hello klusterresurser inte har använt för att i onödan. Men vill du inte också tooignore lösningar som skulle förbättrar hello fördelning av resurser i hello kluster.

hello klustret Resource Manager har två sätt att beräkna kostnader och begränsa dem medan den försöker toomanage hello klustret. hello första mekanism helt enkelt inventering varje gång den blir. Om två lösningar skapas med om hello balansera samma (poäng), och sedan hello klustret Resource Manager föredrar hello något med hello lägsta kostnaden (totalt antal flyttar).

Den här metoden fungerar bra. Men som standard eller statiska laster, är det osannolikt i ett komplext system att alla flyttar är lika. Vissa är sannolikt toobe mycket dyrare.

## <a name="setting-move-costs"></a>Inställningen flytta kostnader 
Du kan ange hello flytta standardkostnaden för en tjänst när den har skapats:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C#: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up hello rest of hello ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Du kan också ange eller uppdatera MoveCost dynamiskt för en tjänst när hello-tjänsten har skapats: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Ange dynamiskt flytta kostnaden på grundval av per replik

hello föregående kodfragment är alla för att ange MoveCost för en hel tjänst samtidigt från själva utanför hello-tjänsten. Dock flytta kostnaden är mest användbara är när hello flytta kostnaden för en specifik tjänstobjekt ändras under dess livslängd. Eftersom hello tjänsterna själva förmodligen ha hello bra uppfattning om hur kostsamma de är toomove en given tidpunkt, det är en API för tjänster tooreport sina egna enskilda flytta kostnad under körningen. 

C#:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Effekten av flytta kostnad
MoveCost har fyra nivåer: noll, lågt, Medium och hög. MoveCosts är relativ tooeach andra, förutom noll. Noll flytta innebär att flytt är ledig och bör inte räknas av mot hello poängen för hello lösning. Inställningen flytten kostnad tooHigh har *inte* garantera hello repliken finns kvar på samma plats.

<center>
![Flytta kostnad som en faktor att välja repliker för flytt][Image1]
</center>

MoveCost hjälper dig att hitta hello lösningar att orsaken hello minst avbrott övergripande och enklaste tooachieve när fortfarande anländer till motsvarande saldo. En tjänst begreppet kostnaden kan vara relativt toomany saker. hello vanligaste faktorer för att beräkna kostnaden flytta är:

- hello mängden tillstånd eller data som hello service har toomove.
- hello kostnaden för frånkoppling av klienter. Flytta en primär replik är vanligtvis kostar mer än hello kostnaden för att flytta en sekundär replik.
- hello kostnaden för att avbryta en pågående åtgärd. Vissa åtgärder på hello data lagra nivå eller åtgärder som utförs i svaret tooa klientanrop är kostsamma. Efter en viss punkt du inte vill toostop dem om du inte behöver. Medan hello igen som händer, öka så hello flytta kostnaden för den här tjänsten objektet tooreduce hello sannolikheten att flyttas. När hello åtgärden är klar kan du ange hello kostnaden tillbaka toonormal.

## <a name="enabling-move-cost-in-your-cluster"></a>Aktivera flytta kostnaden i klustret
För att Hej mer detaljerade MoveCosts toobe beaktas, MoveCost måste vara aktiverat i klustret. Utan den här inställningen hello standardläget för inventering flyttar används för beräkning av MoveCost och MoveCost rapporter ignoreras.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

värdbaserade kluster via ClusterConfig.json för fristående distributioner eller Template.json för Azure:

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

## <a name="next-steps"></a>Nästa steg
- Resurshanteraren för Service Fabric-kluster använder mått toomanage förbrukning och kapacitet i hello kluster. Mer om mått toolearn och hur tooconfigure dem, kolla [hantera resursförbrukning och belastningen i Service Fabric med](service-fabric-cluster-resource-manager-metrics.md).
- toolearn om hur hello klustret Resource Manager hanterar och balanserar belastningen i hello klustret, ta en titt [NLB Service Fabric-kluster](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
