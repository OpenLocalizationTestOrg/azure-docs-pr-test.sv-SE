---
title: "aaaGuidelines & rekommendationer för tillförlitlig samlingar i Azure Service Fabric | Microsoft Docs"
description: "Riktlinjer och rekommendationer för att använda tillförlitliga samlingar för Service Fabric"
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
ms.date: 5/3/2017
ms.author: mcoskun
ms.openlocfilehash: bcdbc9d013bc044e06c43761e7f515c7e4bf340c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Riktlinjer och rekommendationer för tillförlitlig samlingar i Azure Service Fabric
Det här avsnittet innehåller riktlinjer för att använda tillförlitliga Tillståndshanterare och tillförlitlig samlingar. hello målet är toohelp användare undvika vanliga fallgropar.

hello riktlinjer ordnas som enkel rekommendationer prefixet hello villkoren *gör*, *Överväg att*, *Undvik* och *inte*.

* Ändra inte ett objekt av anpassade typer som returneras av läsåtgärder (till exempel `TryPeekAsync` eller `TryGetValueAsync`). Tillförlitliga samlingar, precis som samtidiga samlingar returnerar en referens toohello objekt och inte en kopia.
* Göra djup kopia hello returnerade objekt av en anpassad typ innan du ändrar den. Eftersom strukturer och inbyggda typer är pass-av-värde, behöver inte toodo en djup kopia på dem.
* Använd inte `TimeSpan.MaxValue` för timeout. Timeout ska använda toodetect deadlocks.
* Använd inte en transaktion när den har genomförts, avbröts eller tagits bort.
* Använd inte en uppräkning utanför omfånget för hello transaktionen det skapades i.
* Skapa inte en transaktion i en annan transaktion `using` instruktionen eftersom det kan orsaka deadlocks.
* Att din `IComparable<TKey>` -implementeringen är korrekt. hello system tar beroende på `IComparable<TKey>` för sammanslagning kontrollpunkter och rader.
* Använd uppdateringslås vid läsning av ett objekt med en avsikt tooupdate den tooprevent en viss klass av deadlocks.
* Fundera objekten (till exempel TKey + TValue för tillförlitlig ordlistan) under 80 kilobyte: mindre hello bättre. Detta minskar hello heapen för stora objekt användning samt disk- och -i/o-kraven. Det minskar ofta replikera dubblettdata när bara en liten del av hello värdet uppdateras. Vanliga sätt tooachieve detta i tillförlitliga ordlistan är toobreak rader i toomultiple rader.
* Överväg att använda säkerhetskopiering och återställning funktioner toohave katastrofåterställning.
* Undvika enhet åtgärder och åtgärder för flera enheter (t.ex `GetCountAsync`, `CreateEnumerableAsync`) i hello samma transaktion på grund av toohello olika isoleringsnivåerna tillåter.
* Hantera InvalidOperationException. Användartransaktioner kan avbrytas av hello system för många olika skäl. Till exempel blockerar när hello tillförlitliga Tillståndshanterare ändrar sin roll utanför primära eller när en tidskrävande transaktion trunkering av hello transaktionella logg. I sådana fall kan användare få InvalidOperationException som anger deras transaktionen har redan avslutats. Under förutsättning att, hello avslutning av hello transaktion begärdes inte av hello användaren, bästa sätt toohandle undantaget är toodispose hello transaktion, kontrollera om hello annullering token har har signalerat (eller hello roll hello replik har ändrats), och om inte Skapa en ny transaktion och försök igen.  

Här följer några saker tookeep i åtanke:

* hello standardtidsgränsen är fyra sekunder för alla hello tillförlitliga samling API: er. De flesta användare bör använda hello standardtidsgränsen.
* hello standard annullering token är `CancellationToken.None` i alla tillförlitliga samlingar API: er.
* Hej nyckeltyp parametern (*TKey*) för en tillförlitlig ordlista korrekt måste implementera `GetHashCode()` och `Equals()`. Nycklar måste vara ändras.
* tooachieve hög tillgänglighet för hello tillförlitliga samlingar, varje tjänst bör ha minst ett mål och minsta replikuppsättningen storleken på 3.
* Läsåtgärder på hello sekundära kan läsa versioner som inte är allokerade kvorum.
  Det innebär att en version av data som läses från en enskild sekundär kan vara false nått.
  Läser från primära är alltid stabila: kan aldrig vara false nått.

### <a name="next-steps"></a>Nästa steg
* [Arbeta med Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transaktioner och lås](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Tillförlitliga tillstånd Manager och samling Internals](service-fabric-reliable-services-reliable-collections-internals.md)
* Datahantering
  * [Säkerhetskopiering och återställning](service-fabric-reliable-services-backup-restore.md)
  * [Meddelanden](service-fabric-reliable-services-notifications.md)
  * [Serialisering och uppgradering](service-fabric-application-upgrade-data-serialization.md)
  * [Konfigurationen för hanteraren för tillförlitlig tillstånd](service-fabric-reliable-services-configuration.md)
* Andra
  * [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
  * [För utvecklare för tillförlitlig samlingar](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
