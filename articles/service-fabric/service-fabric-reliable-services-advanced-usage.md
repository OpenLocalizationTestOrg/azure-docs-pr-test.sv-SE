---
title: "aaaAdvanced användning av Reliable Services | Microsoft Docs"
description: "Mer information om avancerad användning av Service Fabric Reliable Services för ökad flexibilitet i dina tjänster."
services: Service-Fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: masnider
ms.assetid: f2942871-863d-47c3-b14a-7cdad9a742c7
ms.service: Service-Fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: e6d6310a4deae9edcfcd76551e1337f0e39e9e5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-usage-of-hello-reliable-services-programming-model"></a>Avancerad användning av hello Reliable Services programmeringsmodellen
Azure Service Fabric gör det enklare att skriva och hantera tillförlitliga tillståndslösa och tillståndskänsliga tjänster. Den här guiden pratar om avancerade användningar av Reliable Services toogain större flexibilitet och kontroll över dina tjänster. Tidigare tooreading detta guide, bekanta dig med [hello Reliable Services programmeringsmodell](service-fabric-reliable-services-introduction.md).

Både tillståndskänsliga och tillståndslösa tjänster har två primära startpunkter för användarkod:

* `RunAsync(C#) / runAsync(Java)`är en generell startpunkt för koden för tjänsten.
* `CreateServiceReplicaListeners(C#)`och `CreateServiceInstanceListeners(C#) / createServiceInstanceListeners(Java)` är för att öppna kommunikationslyssnarna för klientbegäranden.

Dessa två startpunkter räcker för de flesta tjänster. I sällsynta fall när mer kontroll över livscykeln för en tjänst krävs är ytterligare Livscykelhändelser tillgängliga.

## <a name="stateless-service-instance-lifecycle"></a>Tillståndslösa tjänsten instans livscykel
Tillståndslösa tjänsten livscykel är mycket enkelt. Tillståndslösa tjänsten kan bara öppnas, stängts eller avbrutits. `RunAsync`i en tillståndslös tjänst körs när en instans av tjänsten öppnas och avbröts när en tjänstinstans stängts eller avbrutits.

Även om `RunAsync` bör vara tillräckligt nästan alltid hello öppen, Stäng och Avbryt händelser i en tillståndslös tjänst är också tillgängliga:

* `Task OnOpenAsync(IStatelessServicePartition, CancellationToken) - C# / CompletableFuture<String> onOpenAsync(CancellationToken) - Java`OnOpenAsync anropas när hello tillståndslös tjänstinstans är om toobe används. Uppgifter för initiering av utökade tjänsten kan startas just nu.
* `Task OnCloseAsync(CancellationToken) - C# / CompletableFuture onCloseAsync(CancellationToken) - Java`OnCloseAsync anropas när hello tillståndslös tjänstinstans kommer toobe smidigt stängs ned. Detta kan inträffa när hello-Tjänstkod uppgraderas, hello tjänstinstans flyttas på grund av tooload NLB eller ett tillfälligt fel har identifierats. OnCloseAsync kan använda toosafely Stäng några resurser, stoppa behandling i bakgrunden, Slutför sparar externa tillstånd, eller stänga ned befintliga anslutningar.
* `void OnAbort() - C# / void onAbort() - Java`OnAbort anropas när hello tillståndslös tjänstinstans tvång stängs. Detta kallas vanligtvis när ett permanent fel identifieras på hello nod eller Service Fabric inte kan på ett tillförlitligt sätt hantera hello service instans livscykel på grund av toointernal fel.

## <a name="stateful-service-replica-lifecycle"></a>Tillståndskänslig service replik livscykel

> [!NOTE]
> Tillståndskänsliga reliable services stöds inte i Java ännu.
>
>

En tillståndskänslig service replik livscykel är mycket mer komplexa än en instans av tillståndslösa tjänsten. Dessutom tooopen, stänga och avbryta händelser, roll ändringar görs i en tillståndskänslig service replik under dess livslängd. När en tillståndskänslig service replik ändras roll, hello `OnChangeRoleAsync` händelsen utlöses:

* `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`OnChangeRoleAsync anropas när hello tillståndskänslig service replik byter roll, till exempel tooprimary eller sekundär. Primära repliker får skrivstatus (tillåts toocreate och skriva tooReliable samlingar). Sekundära repliker ges Lässtatus (kan bara läsa från befintliga tillförlitliga samlingar). De flesta arbetsobjekt i en tillståndskänslig service utförs med hello primära repliken. Sekundära repliker kan utföra skrivskyddade validering rapportgenerering, datautvinning eller andra skrivskyddade jobb.

I en tillståndskänslig service endast hello primära repliken har skrivbehörighet toostate och är därför vanligtvis när det faktiska arbetet utförs med hello-tjänsten. Hej `RunAsync` metod i en tillståndskänslig service körs endast när hello tillståndskänslig service replik är primär. Hej `RunAsync` metoden avbryts när en primär replik rollen ändringar från primära samt under hello stänga och avbryta händelser.

Med hjälp av hello `OnChangeRoleAsync` händelsen kan du tooperform arbete beroende på replikroll även som svar toorole ändringen.

En tillståndskänslig service ger hello samma fyra Livscykelhändelser som en tillståndslös tjänst hello samma semantik och användningsfall:

```csharp
* Task OnOpenAsync(IStatefulServicePartition, CancellationToken)
* Task OnCloseAsync(CancellationToken)
* void OnAbort()
```

## <a name="next-steps"></a>Nästa steg
Mer avancerade avsnitt relaterade tooService Fabric, finns i hello följande artiklar:

* [Konfigurera tillståndskänsliga Reliable Services](service-fabric-reliable-services-configuration.md)
* [Service Fabric hälsa introduktion](service-fabric-health-introduction.md)
* [Med hjälp av rapporter om hälsotillstånd för felsökning](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Konfigurera Services med hello resurshanteraren för Service Fabric-kluster](service-fabric-cluster-resource-manager-configure-services.md)
