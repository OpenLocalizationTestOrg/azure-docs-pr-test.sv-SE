---
title: "aaaIntroduction tooReliable samlingar i Azure Service Fabric tillståndskänsliga tjänster | Microsoft Docs"
description: "Service Fabric tillståndskänsliga tjänster har tillförlitliga samlingar som gör att du toowrite hög tillgänglighet, skalbara och låg latens molnprogram."
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
ms.openlocfilehash: 9f67c48f13e8b91b84977e127e2545cbb9d9a158
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooreliable-collections-in-azure-service-fabric-stateful-services"></a>Introduktion tooReliable samlingar i Azure Service Fabric tillståndskänsliga tjänster
Tillförlitliga samlingar aktivera toowrite hög tillgänglighet, skalbara och låg latens molnprogram som om du skriver datorprogram. Hej klasser i hello **Microsoft.ServiceFabric.Data.Collections** namnområde ger en uppsättning av samlingar som automatiskt ge ditt tillstånd hög tillgänglighet. Utvecklare behöver tooprogram endast toohello tillförlitliga samling API: er och låta tillförlitliga samlingar hantera hello replikeras och lokala tillstånd.

hello viktiga skillnaden mellan tillförlitliga samlingar och andra tekniker för hög tillgänglighet (till exempel Redis Azure tabelltjänsten och Azure-kötjänsten) är att hello tillstånd sparas lokalt i hello tjänstinstans när också görs högtillgänglig. Detta innebär att:

* Alla Läs är lokala, vilket leder till låg latens och hög genomströmning läser.
* Alla skrivningar innebära hello minsta antal IOs-nätverket, vilket resulterar i låg latens och hög genomströmning skriver.

![Bild av utvecklingen av samlingar.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Tillförlitliga samlingar kan betraktas som hello naturlig utvecklingen av hello **System.Collections** klasser: en ny uppsättning samlingar som har utformats för hello moln och flera datorer program utan att öka komplexiteten för hello-utvecklare. Därför är tillförlitlig samlingar:

* Replikerade: Tillståndsändringar replikeras för hög tillgänglighet.
* Beständiga: Data är beständiga toodisk för hållbarhet mot storskaliga avbrott (till exempel ett datacenter strömavbrott).
* Asynkron: API: er är asynkron tooensure trådar inte blockeras när medför IO.
* Transaktionell: API: er använda hello abstraktion av transaktioner så att du enkelt kan hantera flera tillförlitliga samlingar i en tjänst.

Tillförlitliga samlingar ger stark konsekvens garanterar utanför hello rutan toomake skäl om programtillståndet enklare.
Stark konsekvens uppnås genom att säkerställa incheckningar Slutför förrän hello hela transaktionen har loggats in ett kvorum för merparten av repliker, inklusive hello primära transaktionen.
tooachieve svagare konsekvens, program kan bekräfta att den bakre toohello klient/beställaren innan hello asynkront genomförande returnerar.

hello tillförlitliga samlingar API: er är en utveckling av samtidiga samlingar API: er (i hello **System.Collections.Concurrent** namnområde):

* Asynkron: Returnerar en aktivitet eftersom, till skillnad från samtidiga samlingar hello operations replikeras och beständig.
* Inte out-parametrar: använder `ConditionalValue<T>` tooreturn en boolesk och ett värde i stället för out-parametrar. `ConditionalValue<T>`liknar `Nullable<T>` men kräver inte T toobe en struktur.
* Transaktioner: Använder en transaktion objektet tooenable hello användaråtgärder toogroup på flera tillförlitliga samlingar i en transaktion.

Idag **Microsoft.ServiceFabric.Data.Collections** innehåller tre samlingar:

* [Tillförlitliga ordlista](https://msdn.microsoft.com/library/azure/dn971511.aspx): representerar en replikerad transaktionell och asynkrona mängd nyckel/värde-par. Liknande för**ConcurrentDictionary**, både hello nyckel och hello värdet kan vara av valfri typ.
* [Tillförlitliga kön](https://msdn.microsoft.com/library/azure/dn971527.aspx): representerar en replikerad transaktionell och asynkrona strikt först in, skickas kö. Liknande för**ConcurrentQueue**, hello värdet kan vara av valfri typ.
* [Tillförlitliga samtidiga kön](service-fabric-reliable-services-reliable-concurrent-queue.md): representerar en replikerad transaktionell och asynkrona bästa prestanda ordning kön för högt genomflöde. Liknande toohello **ConcurrentQueue**, hello värdet kan vara av valfri typ.

## <a name="next-steps"></a>Nästa steg
* [Riktlinjer för tillförlitlig samling & rekommendationer](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Arbeta med Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transaktioner och lås](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Tillförlitliga tillstånd Manager och samling Internals](service-fabric-reliable-services-reliable-collections-internals.md)
* Datahantering
  * [Säkerhetskopiering och återställning](service-fabric-reliable-services-backup-restore.md)
  * [Meddelanden](service-fabric-reliable-services-notifications.md)
  * [Reliable Collection-serialisering](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serialisering och uppgradering](service-fabric-application-upgrade-data-serialization.md)
  * [Konfigurationen för hanteraren för tillförlitlig tillstånd](service-fabric-reliable-services-configuration.md)
* Andra
  * [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
  * [För utvecklare för tillförlitlig samlingar](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
