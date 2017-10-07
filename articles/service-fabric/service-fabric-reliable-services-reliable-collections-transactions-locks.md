---
title: "aaaTransactions och låslägen i Azure Service Fabric tillförlitliga samlingar | Microsoft Docs"
description: "Azure Service Fabric tillförlitliga tillstånd Manager och tillförlitlig samlingar transaktioner och låsa."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 340e029aa98f43ad6e46b48f687dad01f9d96f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>Transaktioner och låslägen i Azure Service Fabric tillförlitliga samlingar

## <a name="transaction"></a>Transaktionen
En transaktion är en sekvens med åtgärder som utförs som en logisk enhet på arbetet.
En transaktion måste ha hello följande ACID-egenskaper. (se: https://technet.microsoft.com/en-us/library/ms190612)
* **Odelbarhet**: en transaktion måste vara ett atomic arbetsenheten. Med andra ord antingen alla dataändringar utförs eller ingen av dem har utförts.
* **Konsekvenskontroll**: när du är klar, en transaktion måste lämna alla data i ett konsekvent tillstånd. Alla interna datastrukturer måste vara rätt hello slutet av hello transaktion.
* **Isolering**: ändringar som görs av samtidiga transaktioner måste isoleras från hello ändringar har gjorts av andra samtidiga transaktioner. hello-Isoleringsnivån som används för en åtgärd i en ITransaction bestäms av hello IReliableState utför hello igen.
* **Hållbarhet**: när en transaktion har slutförts effekterna permanent är på plats i hello system. hello ändringar sparas även i hello händelse av ett systemfel.

### <a name="isolation-levels"></a>Isoleringsnivåer
Isoleringsnivå som definierar hello grad toowhich hello transaktion måste isoleras från ändringar som görs av andra transaktioner.
Det finns två isoleringsnivåer som stöds i tillförlitliga samlingar:

* **Upprepbar läsning**: Anger att rapporterna inte kan läsa data som har ändrats, men ännu inte har skickats av andra transaktioner och att inga andra transaktioner kan ändra data som har lästs av hello transaktionen tills hello aktuella transaktionen har slutförts. Mer information finns i [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
* **Ögonblicksbild**: Anger att data läses av någon instruktion i en transaktion är hello transaktionellt konsekvens i hello data som fanns vid hello transaktionens hello början.
  hello transaktion kan identifiera endast dataändringar som har genomförts innan hello hello transaktion.
  Dataändringar har gjorts av andra transaktioner när hello början av hello transaktionen är inte synliga toostatements som körs i hello aktuella transaktionen.
  hello effekten är som om hello-satser i en transaktion hämta en ögonblicksbild av hello Allokerat data som den såg hello början av hello transaktion.
  Ögonblicksbilder är konsekventa för tillförlitlig samlingar.
  Mer information finns i [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Tillförlitliga samlingar väljer automatiskt hello isolering nivå toouse för en viss Läsåtgärd beroende på hello igen och hello roll hello replik för närvarande hello transaktionens skapas.
Följande är hello-tabell som visar isolering nivån standardvärden för tillförlitlig ordlista och kön.

| Åtgärden \ roll | Primär | Sekundär |
| --- |:--- |:--- |
| Läs för enhet |Upprepbar läsning |Ögonblicksbild |
| Uppräkning, antal |Ögonblicksbild |Ögonblicksbild |

> [!NOTE]
> Vanliga exempel för den enda entiteten åtgärder är `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.
> 

Både hello tillförlitliga ordlista och hello tillförlitliga kön stöd för Läs din skriver.
Med andra ord skriva någon i en transaktion synliga tooa följande läses som tillhör toohello samma transaktion.

## <a name="locks"></a>Lås
I tillförlitliga samlingar alla transaktioner implementera rigorösa två fas låsning: en transaktion inte släpptes hello lås som den genererade tills hello transaktion slutar med ett avbrott eller ett genomförande.

Tillförlitliga ordlista använder radnivå låsning för alla åtgärder för samma enhet.
Tillförlitliga kön användning av samtidighet för strikt transaktionella FIFO-egenskapen.
Tillförlitliga kön använder åtgärden nivån Lås så att en transaktion med `TryPeekAsync` och/eller `TryDequeueAsync` och en transaktion med `EnqueueAsync` i taget.
Observera att toopreserve FIFO, om en `TryPeekAsync` eller `TryDequeueAsync` någonsin iakttas att hello tillförlitliga kön är tom, låses också `EnqueueAsync`.

Skriva operations alltid vidta exklusivt lås.
För läsåtgärder beror hello låsning på ett antal faktorer.
Alla skrivskyddade åtgärd utfördes med ögonblicksbildisolering är låsfri.
Alla repeterbara Läsåtgärd som standard tar delade Lås.
Men för alla skrivskyddade åtgärder som stöder Upprepbar läsning kan hello användare begära något uppdateringslås i stället för hello delade låset.
Något uppdateringslås är ett asymmetriskt Lås används tooprevent en gemensam form av dödläge som uppstår när flera transaktioner låsa resurser för potentiella uppdateringar vid ett senare tillfälle.

hello Lås kompatibilitetsmatrix finns i följande tabell hello:

| Begära \ beviljas | Ingen | Delad | Uppdatering | Exklusivt |
| --- |:--- |:--- |:--- |:--- |
| Delad |Ingen konflikt |Ingen konflikt |Konflikt |Konflikt |
| Uppdatering |Ingen konflikt |Ingen konflikt |Konflikt |Konflikt |
| Exklusivt |Ingen konflikt |Konflikt |Konflikt |Konflikt |

Timeout-argument i hello tillförlitliga samlingar API: er används för identifiering av dödläge.
Två transaktioner (T1 och T2) försöker tooread och uppdatera K1.
Det är möjligt för dem toodeadlock, eftersom de båda hamna med hello delade låset.
I det här fallet gör en eller båda av hello operations timeout.

Det här scenariot för deadlock är ett bra exempel på hur ett uppdateringslås kan förhindra deadlocks.

## <a name="next-steps"></a>Nästa steg
* [Arbeta med Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Reliable Services-meddelanden](service-fabric-reliable-services-notifications.md)
* [Tillförlitlig säkerhetskopiering och återställning (återställning)](service-fabric-reliable-services-backup-restore.md)
* [Konfigurationen för hanteraren för tillförlitlig tillstånd](service-fabric-reliable-services-configuration.md)
* [För utvecklare för tillförlitlig samlingar](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

