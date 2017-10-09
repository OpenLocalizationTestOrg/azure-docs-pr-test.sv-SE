---
title: "aaaAzure Service Fabric-prestandaövervakning | Microsoft Docs"
description: "Mer information om prestandaräknare för övervakning och diagnostik av Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 54d4c62b7250a1f70b0898ba07ae5a37716f4cf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-metrics"></a>Prestandamått

Mått måste vara insamlade toounderstand hello prestanda på klustret samt hello-program som körs i den. För Service Fabric-kluster rekommenderar vi att samla in hello följande prestandaräknare.

## <a name="nodes"></a>Noder

Överväg att samla in följande prestandaräknare toobetter förstå hello belastningen på varje dator och att rätt kluster skalning beslut hello hello i klustret, datorer.

| Räknaren kategori | Räknarens namn |
| --- | --- |
| Fysisk disk (per Disk) | Genomsn. Läs diskkölängd |
| Fysisk disk (per Disk) | Genomsn. Diskkölängd för skrivning |
| Fysisk disk (per Disk) | Genomsn. Disk sek/läsning |
| Fysisk disk (per Disk) | Genomsn. Disk sek/skrivning |
| Fysisk disk (per Disk) | Diskläsningar/sek |
| Fysisk disk (per Disk) | Disk-lästa byte/sek |
| Fysisk disk (per Disk) | Diskskrivningar/sek |
| Fysisk disk (per Disk) | Disk-skrivna byte/s |
| Minne | Tillgängliga megabyte |
| Växling fil | % Användning |
| Process(total) | % Processortid |
| Processen (per service) | % Processortid |
| Processen (per service) | Process-ID |
| Processen (per service) | Privata byte |
| Processen (per service) | Antal tråd |
| Processen (per service) | Virtuell storlek-byte |
| Processen (per service) | Sidmängd |
| Processen (per service) | Privat arbetsminne |

## <a name="net-applications-and-services"></a>.NET-program och tjänster

Samla in hello följande räknare om du distribuerar .NET services tooyour klustret. 

| Räknaren kategori | Räknarens namn |
| --- | --- |
| .NET CLR-minne (per service) | Process-ID |
| .NET CLR-minne (per service) | # Totalt antal allokerade byte |
| .NET CLR-minne (per service) | # Totalt antal reserverade byte |
| .NET CLR-minne (per service) | # Byte i alla Heaps |
| .NET CLR-minne (per service) | Antal generation 0-insamlingar |
| .NET CLR-minne (per service) | Antal generation 1-samlingar |
| .NET CLR-minne (per service) | # Generation 2-insamlingar |
| .NET CLR-minne (per service) | Tid i GC i procent |

### <a name="service-fabrics-custom-performance-counters"></a>Service Fabric anpassade prestandaräknare

Service Fabric genererar en stor mängd anpassade prestandaräknare. Om du har hello SDK är installerat, du kan se hello omfattande lista på din Windows-dator i tillämpningsprogrammet Prestandaövervakaren (Start > Prestandaövervakaren). 

Du distribuerar tooyour klustret i hello program, om du använder Reliable Actors, lägger du till countes från `Service Fabric Actor` och `Service Fabric Actor Method` kategorier (se [Service Fabric tillförlitliga aktörer diagnostik](service-fabric-reliable-actors-diagnostics.md)).

Om du använder Reliable Services på samma sätt har vi `Service Fabric Service` och `Service Fabric Service Method` räknaren kategorier som du bör samla in prestandaräknare från. 

Om du använder tillförlitliga samlingar, rekommenderar vi att lägga till hello `Avg. Transaction ms/Commit` från hello `Service Fabric Transactional Replicator` toocollect hello genomsnittlig commit latens per transaktion mått.


## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [händelse genereras på hello plattformsnivå](service-fabric-diagnostics-event-generation-infra.md) i Service Fabric
* Samla in prestandavärden via [Azure-diagnostik](service-fabric-diagnostics-event-aggregation-wad.md)
