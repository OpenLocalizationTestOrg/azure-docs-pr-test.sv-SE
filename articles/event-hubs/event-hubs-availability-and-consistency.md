---
title: "aaaAvailability och konsekvens i Händelsehubbar i Azure | Microsoft Docs"
description: "Hur tooprovide hello maximal mängd tillgänglighet och konsekvens med Händelsehubbar i Azure med hjälp av partitioner."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a8ededaae1589830da21cb8910ca694d2d628bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a>Tillgänglighet och konsekvens i Händelsehubbar

## <a name="overview"></a>Översikt
Händelsehubbar i Azure använder en [partitionering modellen](event-hubs-features.md#partitions) tooimprove tillgänglighet och parallellisering inom en enskild händelsehubb. Till exempel om en händelsehubb har fyra partitioner och en av dessa partitioner flyttas från en server tooanother i en åtgärd för belastningsutjämning, du fortfarande skicka och ta emot från tre partitioner. Dessutom med fler partitioner kan du toohave flera samtidiga läsare bearbetning av dina data, förbättra din sammanställda genomflöde. Förstå hello följderna av att partitionering och ordning i ett distribuerat system är en viktig aspekt för lösningen.

toohelp förklarar hello säkerhetsaspekten ordning och tillgänglighet, se hello [CAP-sats](https://en.wikipedia.org/wiki/CAP_theorem), även kallad Brewers-sats. Den här sats beskrivs hello val mellan konsekvens, tillgänglighet och tolerans för partitionen.

Brewers sats definierar konsekvens och tillgänglighet på följande sätt:
* Partitionera tolerans: hello möjligheten för en databearbetning system toocontinue databearbetning, även om en partition fel inträffar.
* Tillgänglighet: en icke-misslyckas nod returnerar ett rimligt svar inom en rimlig tidsperiod (med några fel eller timeout).
* Konsekvenskontroll: Läs garanteras tooreturn hello senaste skrivåtgärder för en viss klient.

## <a name="partition-tolerance"></a>Tolerans för partition
Händelsehubbar är byggt på en partitionerad datamodell. Du kan konfigurera hello antalet partitioner i din event hub under installationen, men du kan inte ändra det här värdet senare. Eftersom du måste använda partitioner med Händelsehubbar kan ha du toomake beslut om tillgänglighet och konsekvens för ditt program.

## <a name="availability"></a>Tillgänglighet
hello enklaste sättet tooget igång med Händelsehubbar är toouse hello standardbeteendet. Om du skapar en ny `EventHubClient` objekt och använda hello `Send` metoden händelserna automatiskt distribueras mellan partitioner i din event hub. Detta gör att hello största mängden tid.

Den här modellen är prioriterade för användningsområden som kräver hello maximal tid.

## <a name="consistency"></a>Konsekvens
I vissa situationer kan det vara viktigt att hello sorteringen av händelser. Du kanske till exempel din serverdel system tooprocess ett uppdateringskommando innan ett borttagningskommando. I den här instansen, du kan ange hello partitionsnyckel på en händelse eller Använd en `PartitionSender` objekt tooonly skicka händelser tooa viss partition. På så sätt att när dessa händelser läses från hello partition, är de läsas i ordning.

Kom ihåg att om hello viss partition toowhich som du skickar är tillgänglig, visas ett felsvar med den här konfigurationen. Som en jämförelsepunkt om du inte har en mappning mellan tooa enskild partition, skickar hello händelsehubbtjänsten din händelse toohello nästa tillgängliga partition.

En möjlig lösning tooensure ordning när också maximera upptid, skulle vara tooaggregate händelser som en del av programmet för händelsebearbetning. Hej enklaste sättet tooaccomplish detta är toostamp din händelse med en anpassad sequence number-egenskapen. hello följande kod visar ett exempel:

```csharp
// Get hello latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

Det här exemplet skickar din händelse tooone över hello tillgängliga partitioner i din event hub och anger hello motsvarande sekvensnumret från ditt program. Denna lösning kräver tillstånd toobe som tillämpningsprogrammet bearbetning, men ger en slutpunkt som är mer troligt toobe som är tillgängliga för din avsändare.

## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Översikt över Event Hubs service](event-hubs-what-is-event-hubs.md)
* [Skapa en Event Hub](event-hubs-create.md)
