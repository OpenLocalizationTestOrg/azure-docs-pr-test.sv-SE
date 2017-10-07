---
title: "aaaActors diagnostik och övervakning | Microsoft Docs"
description: "Den här artikeln beskriver hello diagnostik- och prestandaövervakning funktioner i hello Service Fabric Reliable Actors körning, inklusive hello händelser och prestandaräknare som sänds av den."
services: service-fabric
documentationcenter: .net
author: abhishekram
manager: timlt
editor: vturecek
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: abhisram
ms.openlocfilehash: 5b266d67875722feef5c5be8861bda6d8132a7d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnostik- och prestandaövervakning för Reliable Actors
hello Reliable Actors runtime avger [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) händelser och [prestandaräknare](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Dessa ger insikter om hur hello runtime fungerar och hjälp med felsökning och övervakning av programprestanda.

## <a name="eventsource-events"></a>EventSource händelser
hello EventSource providernamn för hello Reliable Actors runtime är ”Microsoft-ServiceFabric-aktörer”. Händelser från den här händelsekällan visas i hello [diagnostikhändelser](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) fönstret när hello aktören programmet [felsökas i Visual Studio](service-fabric-debugging-your-application.md).

Exempel på Verktyg och tekniker som hjälper till att samla in och/eller visa EventSource händelser är [förhandsgranskning](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), [semantiska loggning](https://msdn.microsoft.com/library/dn774980.aspx), och hello [ Microsofts bibliotek för TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Nyckelord
Alla händelser som hör toohello tillförlitliga aktörer EventSource är associerade med ett eller flera nyckelord. På så sätt kan du filtrera händelser som samlas in. hello efter nyckelordet bits har definierats.

| bitar | Beskrivning |
| --- | --- |
| 0x1 |En uppsättning viktiga händelser som summerar hello åtgärden hello aktörer Fabric Runtime. |
| 0x2 |Uppsättningen av händelser som beskriver aktören metodanrop. Mer information finns i hello [inledande avsnittet på aktörer](service-fabric-reliable-actors-introduction.md). |
| 0x4 |En uppsättning händelser relaterade tooactor tillstånd. Mer information finns i avsnittet hello på [aktören tillståndshantering](service-fabric-reliable-actors-state-management.md). |
| 0x8 |Uppsättning händelser relaterade tooturn-baserade samtidighet i hello aktören. Mer information finns i avsnittet hello på [samtidighet](service-fabric-reliable-actors-introduction.md#concurrency). |

## <a name="performance-counters"></a>Prestandaräknare
hello Reliable Actors runtime definierar hello efter prestandaräknarkategorier.

| Kategori | Beskrivning |
| --- | --- |
| Service Fabric aktören |Räknare för specifika tooAzure Service Fabric aktörer, t.ex. tidsåtgång toosave aktörstillstånd |
| Service Fabric-Aktörsmetod |Räknare för specifika toomethods som implementeras av Service Fabric aktörer, t.ex. hur ofta en aktören-metoden har anropats |

Varje hello ovanför kategorier har en eller flera räknare.

Hej [Windows Prestandaövervakaren](https://technet.microsoft.com/library/cc749249.aspx) program som är tillgängligt som standard i hello Windows operativsystem kan vara används toocollect och visa information om prestandaräknare. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) är ett annat alternativ för att samla in prestandaräknardata och överföra den tooAzure tabeller.

### <a name="performance-counter-instance-names"></a>Namn på prestandaräknarinstanser
Ett kluster med ett stort antal aktörstjänster eller aktören service partitioner har ett stort antal aktören prestandaräknaren instanser. hello namn på prestandaräknarinstanser hjälper dig identifiera hello specifika [partition](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) och aktörsmetod (om tillämpligt) som hello prestandaräknarinstans är associerad med.

#### <a name="service-fabric-actor-category"></a>Service Fabric aktören kategori
För hello kategori `Service Fabric Actor`, hello namn på prestandaräknarinstanser finns i hello följande format:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* är hello strängrepresentation av hello Service Fabric partitions-ID som hello prestandaräknarinstans är associerad med. hello partitions-ID är ett GUID och dess strängrepresentation genereras via hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metod med Formatspecificeraren ”D”.

*ActorRuntimeInternalID* är hello strängrepresentation av ett 64-bitars heltal som genereras av hello aktörer Fabric runtime för intern användning. Detta ingår i hello prestandaräknarinstans namn tooensure dess unika och undvika konflikter med andra namn på prestandaräknarinstanser. Användarna bör inte försöka toointerpret den här delen av hello namn på prestandaräknare instans.

hello följande är ett exempel på ett räknaren instansnamn för en räknare som tillhör toohello `Service Fabric Actor` kategori:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

I hello-exemplet ovan, `2740af29-78aa-44bc-a20b-7e60fb783264` är hello strängrepresentation av hello Service Fabric partitions-ID och `635650083799324046` är hello 64-bitars-ID som genereras för hello runtime internt använder.

#### <a name="service-fabric-actor-method-category"></a>Service Fabric-Aktörsmetod kategori
För hello kategori `Service Fabric Actor Method`, hello namn på prestandaräknarinstanser finns i hello följande format:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* hello namnet på hello-aktörsmetod som hello prestandaräknarinstans associeras med. hello format för metodnamn hello bestäms baserat på vissa logiken i hello aktörer Fabric runtime som balanserar hello läsbarhet hello namn med begränsningar på hello högst hello namn på prestandaräknarinstanser i Windows.

*ActorsRuntimeMethodId* är hello strängrepresentation av en 32-bitars heltal som genereras av hello aktörer Fabric runtime för intern användning. Detta ingår i hello prestandaräknarinstans namn tooensure dess unika och undvika konflikter med andra namn på prestandaräknarinstanser. Användarna bör inte försöka toointerpret den här delen av hello namn på prestandaräknare instans.

*ServiceFabricPartitionID* är hello strängrepresentation av hello Service Fabric partitions-ID som hello prestandaräknarinstans är associerad med. hello partitions-ID är ett GUID och dess strängrepresentation genereras via hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metod med Formatspecificeraren ”D”.

*ActorRuntimeInternalID* är hello strängrepresentation av ett 64-bitars heltal som genereras av hello aktörer Fabric runtime för intern användning. Detta ingår i hello prestandaräknarinstans namn tooensure dess unika och undvika konflikter med andra namn på prestandaräknarinstanser. Användarna bör inte försöka toointerpret den här delen av hello namn på prestandaräknare instans.

hello följande är ett exempel på ett räknaren instansnamn för en räknare som tillhör toohello `Service Fabric Actor Method` kategori:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

I hello-exemplet ovan, `ivoicemailboxactor.leavemessageasync` är hello metodnamnet `2` är hello 32-bitars-ID som genererades för hello runtime internt använder `89383d32-e57e-4a9b-a6ad-57c6792aa521` är hello strängrepresentation av hello Service Fabric partitions-ID och `635650083804480486` är hello 64-bitars-ID genereras för hello runtime internt bruk.

## <a name="list-of-events-and-performance-counters"></a>Lista över händelser och prestandaräknare
### <a name="actor-method-events-and-performance-counters"></a>Aktören metoden händelser och prestandaräknare
hello Reliable Actors runtime avger hello följande händelser relaterade för[aktören metoder](service-fabric-reliable-actors-introduction.md).

| händelsenamnet | Händelse-ID | Nivå | Nyckelordet | Beskrivning |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Utförlig |0x2 |Aktörer runtime är om tooinvoke en aktörsmetod. |
| ActorMethodStop |8 |Utförlig |0x2 |En aktörsmetod har avslutats. Som är hello runtime asynkront anrop toohello aktörsmetod har returnerat och hello aktiviteten som returneras av hello-aktörsmetod har slutförts. |
| ActorMethodThrewException |9 |Varning |0x3 |Ett undantag uppstod under hello körningen av en aktörsmetod under hello runtime asynkront anrop toohello aktörsmetod eller vid hello körning av hello uppgiften som returneras av hello-aktörsmetod. Den här händelsen indikerar någon form av fel i hello aktören kod som behöver undersökning. |

hello Reliable Actors runtime publicerar hello efter Prestandaräknare relaterade toohello aktören metoder.

| Kategorinamn | Räknarens namn | Beskrivning |
| --- | --- | --- |
| Service Fabric-Aktörsmetod |Anrop/sek |Antalet gånger som hello aktörstjänstmetoden anropas per sekund |
| Service Fabric-Aktörsmetod |Genomsnittlig tid i millisekunder per anrop |Tidsåtgång tooexecute hello aktörstjänstmetoden i millisekunder |
| Service Fabric-Aktörsmetod |Undantag/sek |Antalet gånger som hello aktörstjänstmetoden utlöste ett undantag per sekund |

### <a name="concurrency-events-and-performance-counters"></a>Concurrency-händelser och prestandaräknare
hello Reliable Actors runtime avger hello följande händelser relaterade för[samtidighet](service-fabric-reliable-actors-introduction.md#concurrency).

| händelsenamnet | Händelse-ID | Nivå | Nyckelordet | Beskrivning |
| --- | --- | --- | --- | --- |
| ActorMethodCallsWaitingForLock |12 |Utförlig |0x8 |Den här händelsen skrivs hello början av varje ny Stäng i en aktör. Den innehåller hello antal väntande aktörsanrop som väntar på tooacquire hello lås per aktör som tillämpar bygger samtidighet. |

hello Reliable Actors runtime publicerar hello efter Prestandaräknare relaterade tooconcurrency.

| Kategorinamn | Räknarens namn | Beskrivning |
| --- | --- | --- |
| Service Fabric aktören |Antal aktörsanrop som väntar på aktörslås |Antalet väntande aktörsanrop som väntar på tooacquire hello lås per aktör som tillämpar bygger samtidighet |
| Service Fabric aktören |Genomsnittlig låsväntetid i millisekunder |Tid vidtas (i millisekunder) tooacquire hello lås per aktör som tillämpar bygger samtidighet |
| Service Fabric aktören |Genomsnittlig tid i millisekunder för aktörslåsaktivering |Tid (i millisekunder) för vilka hello lås per aktör hålls |

### <a name="actor-state-management-events-and-performance-counters"></a>Aktören management händelser och prestandaräknare
hello Reliable Actors runtime avger hello följande händelser relaterade för[aktören tillståndshantering](service-fabric-reliable-actors-state-management.md).

| händelsenamnet | Händelse-ID | Nivå | Nyckelordet | Beskrivning |
| --- | --- | --- | --- | --- |
| ActorSaveStateStart |10 |Utförlig |0x4 |Aktörer runtime är om toosave hello aktörstillstånd. |
| ActorSaveStateStop |11 |Utförlig |0x4 |Aktörer runtime är klar sparar hello aktörstillstånd. |

hello Reliable Actors runtime publicerar hello följande prestandahantering räknare relaterade tooactor tillstånd.

| Kategorinamn | Räknarens namn | Beskrivning |
| --- | --- | --- |
| Service Fabric aktören |Genomsnittlig tid i millisekunder per Spara tillstånd-åtgärd |Tidsåtgång toosave aktörstillstånd i millisekunder |
| Service Fabric aktören |Genomsnittlig tid i millisekunder per Läs in tillstånd-åtgärd |Tidsåtgång tooload aktörstillstånd i millisekunder |

### <a name="events-related-tooactor-replicas"></a>Händelser relaterade tooactor repliker
hello Reliable Actors runtime avger hello följande händelser relaterade för[aktören repliker](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

| händelsenamnet | Händelse-ID | Nivå | Nyckelordet | Beskrivning |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoleToPrimary |1 |Information |0x1 |Aktören replik ändras rollen tooPrimary. Detta innebär att hello aktörer för den här partitionen kommer att skapas i den här repliken. |
| ReplicaChangeRoleFromPrimary |2 |Information |0x1 |Aktören replik ändras rollen toonon-primära. Detta innebär att hello aktörer för den här partitionen inte längre kommer att skapas i den här repliken. Inga nya begäranden levereras tooactors som redan har skapats i den här repliken. hello aktörer kommer att raderas när alla pågående begäranden har slutförts. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktören aktivering och inaktivering av händelser och prestandaräknare
hello Reliable Actors runtime avger hello följande händelser relaterade för[aktören aktivering och inaktivering av](service-fabric-reliable-actors-lifecycle.md).

| händelsenamnet | Händelse-ID | Nivå | Nyckelordet | Beskrivning |
| --- | --- | --- | --- | --- |
| ActorActivated |5 |Information |0x1 |En aktör har aktiverats. |
| ActorDeactivated |6 |Information |0x1 |En aktör har inaktiverats. |

hello Reliable Actors runtime publicerar hello följande Prestandaräknare relaterade tooactor aktivering och inaktivering.

| Kategorinamn | Räknarens namn | Beskrivning |
| --- | --- | --- |
| Service Fabric aktören |Genomsnittlig tid i millisekunder OnActivateAsync |Tid tooexecute OnActivateAsync-metoden i millisekunder |

### <a name="actor-request-processing-performance-counters"></a>Aktören begäranbearbetningen prestandaräknare
När en klient anropar en metod via ett aktören proxy-objekt, resulterar det i ett meddelande om begäran som skickas via hello nätverkstjänsten toohello aktören. hello-tjänsten bearbetar begäran hello-meddelande och skickar en svar tillbaka toohello klient. hello Reliable Actors runtime publicerar hello efter Prestandaräknare relaterade tooactor behandling.

| Kategorinamn | Räknarens namn | Beskrivning |
| --- | --- | --- |
| Service Fabric aktören |Antal väntande förfrågningar |Antalet begäranden som bearbetas i hello-tjänsten |
| Service Fabric aktören |Genomsnittlig tid i millisekunder per begäran |Tid (i millisekunder) som hello service tooprocess en begäran |
| Service Fabric aktören |Genomsnittlig tid i millisekunder för deserialiseringsbegäran |Tid vidtas (i millisekunder) toodeserialize aktören meddelandet med begäran när den tas emot med hello-tjänsten |
| Service Fabric aktören |Genomsnittlig tid i millisekunder för serialiseringssvar |Svarsmeddelandet för tid vidtas (i millisekunder) tooserialize hello aktören på hello-tjänsten innan hello svar skickas toohello klienten |

## <a name="next-steps"></a>Nästa steg
* [Hur Reliable Actors använda hello Service Fabric-plattformen](service-fabric-reliable-actors-platform.md)
* [Aktören API-referensdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Exempelkod](https://github.com/Azure/servicefabric-samples)
* [EventSource providrar på förhandsgranskning](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
