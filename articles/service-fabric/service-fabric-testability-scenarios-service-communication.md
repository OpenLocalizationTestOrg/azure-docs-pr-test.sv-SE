---
title: "Möjlighet att testa: Tjänsten kommunikation | Microsoft Docs"
description: "Service-to-service-kommunikation är en kritisk integrering av ett Service Fabric-program. Den här artikeln beskrivs överväganden vid utformning och tester tekniker."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 017557df-fb59-4e4a-a65d-2732f29255b8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 4a8f941c1e8e641384a9ee3a1149dabaaf9983cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric möjlighet att testa scenarier: tjänsten kommunikation
Mikrotjänster och tjänstorienterad arkitektur formatmallar ytan naturligt i Azure Service Fabric. I dessa typer av distribuerade arkitekturer består komponentbaserade mikrotjänster program vanligtvis av flera tjänster som behöver tootalk tooeach andra. I även hello enklaste fall kan har du vanligtvis minst en tillståndslös webbtjänst och en tillståndskänslig data storage-tjänst som behöver toocommunicate.

Service-to-service-kommunikation är en kritisk integrering av ett program, eftersom varje tjänst Exponerar en fjärransluten tooother API-tjänster. Arbeta med en uppsättning API-gränser som innebär att i/o vanligtvis kräver viss precision med mycket testning och validering.

Det finns många överväganden toomake när dessa gränser för tjänsten wired tillsammans i ett distribuerat system:

* *Transport-protokollet*. Ska du använda HTTP för ökad samverkan eller anpassade binära protokoll för maximalt dataflöde?
* *Felhantering*. Hur permanent och tillfälliga fel hanteras? Vad händer när en tjänst flyttar tooa annan nod?
* *Tidsgränser och fördröjning*. I multitiered program, hur varje tjänstnivå hanterar latens via hello stacken och toohello användare?

Om du använder en av hello inbyggd kommunikation tjänstkomponenter som tillhandahålls av Service Fabric eller du skapar egna, är testa hello samverkan mellan dina tjänster kritiska tooensuring återhämtning i ditt program.

## <a name="prepare-for-services-toomove"></a>Förbereda för tjänster toomove
Instanser av tjänsten kan flytta över tid. Detta gäller särskilt när de är konfigurerade med belastningen anpassad skräddarsydda optimal Resursanvändning belastningsutjämning. Service Fabric flyttar din tjänst instanser toomaximize deras tillgänglighet även under uppgraderingar, redundans, skalbara och andra situationer som sker över hello livslängden för ett distribuerat system.

När tjänster flyttar i hello kluster, ska klienterna och andra tjänster vara förberedda toohandle två scenarier när de talar tooa tjänsten:

* hello service-instans eller partition replik har flyttats sedan hello senaste gången som du har talat tooit. Detta är en normal del av en livscykel för tjänsten och det bör vara förväntade toohappen under hello livslängd för programmet.
* hello service-instans eller partition repliken är i hello processen att flytta. Även om växling vid fel på en tjänst från en nod tooanother uppstår mycket snabbt i Service Fabric kan finnas det en fördröjning i tillgänglighet om hello kommunikation del av din tjänst är långsam toostart.

Hantering av dessa scenarier smidigt är viktigt för ett system för smooth körs. toodo så Tänk som:

* Varje tjänst som kan vara anslutna toohas en *adress* att den lyssnar på (till exempel http- eller WebSockets). När du flyttar en tjänstinstans eller partition, ändrar sin adress-slutpunkt. (Den flyttas tooa annan nod med en annan IP-adress.) Om du använder hello inbyggd kommunikationskomponenter hanterar de nytt lösa postadresser åt dig.
* Det kan finnas en tillfällig ökning i tjänsten svarstid som hello service-instans startar upp dess lyssnare igen. Detta beror på hur snabbt hello tjänsten öppnar hello lyssnare när hello tjänstinstans flyttas.
* Alla befintliga anslutningar måste toobe stänga och öppna när hello service öppnas på en ny nod. En korrekt nodavstängning eller omstart ger dig tid för befintliga anslutningar toobe avslutas på vanligt sätt.

### <a name="test-it-move-service-instances"></a>Testa den: flytta instanser av tjänsten
Med hjälp av Service Fabric datatillgång verktyg kan du skapa en test-scenario tootest situationerna på olika sätt:

1. Flytta en tillståndskänslig service primära repliken.
   
    hello kan primär replik av en tillståndskänslig service-partitionen flyttas av olika orsaker. Använd den här tootarget hello primära repliken av en specifik partition toosee hur dina tjänster reagerar toohello flytta på ett mycket kontrollerat sätt.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Stoppa en nod.
   
    När en nod har stoppats hello Service Fabric flyttar alla hello service instanser eller partitioner som fanns på den noden tooone av andra tillgängliga noder i klustret hello. Använd den här tootest en situation där en nod har förlorats från ditt kluster och alla instanser av tjänsten hello och repliker på noden har toomove.
   
    Du kan stoppa en nod med hjälp av hello PowerShell **stoppa ServiceFabricNode** cmdlet:
   
    ```powershell
   
    PS > Restart-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Underhåll tjänsttillgänglighet för
Service Fabric är utformad tooprovide hög tillgänglighet för dina tjänster som en plattform. Men i extrema fall underliggande infrastruktur kan fortfarande ha inte finns. Det är viktigt tootest för dessa scenarier för.

Tillståndskänsliga tjänster använder ett kvorum-baserat system tooreplicate tillstånd för hög tillgänglighet. Det innebär att ett kvorum av repliker måste toobe tillgängliga tooperform skrivåtgärder. Ett kvorum av repliker kanske inte tillgänglig i sällsynta fall, till exempel ett omfattande maskinvarufel. I dessa fall kan du inte kan tooperform skrivåtgärder, men du kommer fortfarande att kunna tooperform läsåtgärder.

### <a name="test-it-write-operation-unavailability"></a>Testa den: skriva åtgärden otillgänglighet
Med hello datatillgång i Service Fabric kan du mata in ett fel som startar förlorar kvorum som ett test. Även om ett sådant scenario är sällsynt, är det viktigt att klienter och tjänster som är beroende av en tillståndskänslig service förberetts toohandle situationer där denne inte fatta skriva begäranden tooit. Det är också viktigt att hello tillståndskänslig själva tjänsten känner till denna möjlighet och smidigt kommunicera den toocallers.

Du kan ge upphov till förlorar kvorum med hjälp av hello PowerShell **Invoke-ServiceFabricPartitionQuorumLoss** cmdlet:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

I det här exemplet anger vi `QuorumLossMode` för`QuorumReplicas` tooindicate som vi vill tooinduce kvorumförlust utan att stoppa alla repliker. Det här sättet läsåtgärder är fortfarande möjligt. tootest ett scenario där en hel partition är tillgänglig, kan du ange den här växeln för`AllReplicas`.

## <a name="next-steps"></a>Nästa steg
[Mer information om datatillgång åtgärder](service-fabric-testability-actions.md)

[Mer information om möjlighet att testa scenarier](service-fabric-testability-scenarios.md)

