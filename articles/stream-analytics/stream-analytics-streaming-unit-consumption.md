---
title: 'Azure Stream Analytics: Optimera ditt jobb toouse Streaming enheter effektivt | Microsoft Docs'
description: "Fråga bästa praxis för skalning och prestanda i Azure Stream Analytics."
keywords: "Streaming unit frågeprestanda"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 5ad98b34d625190a879260f54c9eff0294e230cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-job-toouse-streaming-units-efficiently"></a>Optimera din jobbet toouse Streaming enheter effektivt

Azure Stream Analytics aggregerar hello prestanda ”vikt” för ett jobb som körs i enheter för strömning (SUs). SUs representerar hello datorresurser som förbrukade tooexecute ett jobb. SUs ge ett sätt toodescribe hello relativa händelsebearbetning kapacitet baserat på ett blandat mått av CPU, minne, läser och skriver priser. Den här kapaciteten kan du fokusera på hello frågan logik och tar bort du inte behöver tooknow lagring nivå prestandaöverväganden, allokera minne för ditt jobb manuellt och ungefärlig hello CPU core-antal behövs toorun ditt jobb inom rimlig tid.

## <a name="how-many-sus-are-required-for-a-job"></a>Hur många SUs krävs för ett jobb?

Väljer hello antalet nödvändiga SUs för ett visst jobb beror på hello partition konfiguration för hello indata och hello-fråga som har definierats inom hello jobb. Hej **skala** bladet kan du tooset hello rätt antal SUs. Det är en god rutin tooallocate flera SUs än vad som behövs. hello Stream Analytics-bearbetningsmotorn optimeras för svarstid och genomströmning till hello kostnaden för att allokera ytterligare minne.

I allmänhet hello bästa praxis är toostart med 6 SUs för frågor som inte använder *PARTITION BY*. Sedan fastställa hello av platsen genom att använda en prövningsmetod med där du ändra hello antal SUs när du skickar representativt datamängder och undersöka hello SU % användning mått.

Azure Stream Analytics behåller händelser i ett fönster som kallas hello ”beställning buffert” innan den börjar eventuell bearbetning. Händelser är sorterade i hello ordna om fönster med tid och efterföljande åtgärder utförs för närvarande sorteras hello-händelser. Ordningen händelser vid tidpunkten säkerställer att hello-operator har insyn i alla hello händelser i hello anges tidsperioden. Du kan också hello operatorn utför hello nödvändiga bearbetning och ger utdata. En sidoeffekt av denna metod är att bearbetningen försenas hello varaktighet hello ordna om fönster. hello minneskrav för hello jobb (som påverkar SU förbrukning) är en funktion av hello storleken på den här ordna om fönster och hello antalet händelser som finns i den.

> [!NOTE]
> När hello antalet läsare ändras under jobbet uppgraderingar, skrivs tillfälligt varningar tooaudit loggar. Stream Analytics-jobb återskapa automatiskt från dessa tillfälliga problem.

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a>Vanliga orsaker till hög minne för SU hårt för att köra jobb

### <a name="high-cardinality-for-group-by"></a>Hög kardinalitet för GROUP BY

Hej Kardinaliteten för inkommande händelser avgör minnesanvändning för hello jobb.

Till exempel i `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hello nummer som är associerat med **klustrade** är hello kardinalitet för hello fråga.

toomitigate problem som orsakas av hög kardinalitet skala ut hello frågan genom att öka partitioner som använder **PARTITION BY**.

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

Hej antalet *klustrade* är hello kardinalitet för GROUP BY här.

När hello frågan är partitionerad den som är utspridda över flera noder. Därför minskas hello antalet händelser som kommer till varje nod, vilket minskar i sin tur hello storleken på hello beställning buffert. Du bör också partitionera event hub partitioner av partitionid.

### <a name="high-unmatched-event-count-for-join"></a>Hög omatchat antal för koppling

hello antal omatchade händelser i en koppling påverkar hello minnesanvändning hello frågan. Till exempel ta en fråga som söker efter toofind hello antal exponeringar som genererar klick:

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

Det är möjligt att många annonser visas och några klickningar skapas i det här scenariot. Om detta inträffar kräver hello jobbet tookeep alla hello händelser inom hello tidsperioden. Hej mängden minne som används är proportionell toohello fönstret storlek och händelsen hastighet. 

toomitigate den här situationen kan skala ut hello frågan genom att öka partitioner genom att använda PARTITION. 

När hello frågan är partitionerad den som är utspridda över flera noder för bearbetning. Därför minskas hello antalet händelser som kommer till varje nod, vilket minskar i sin tur hello storleken på hello beställning buffert.

### <a name="large-number-of-out-of-order-events"></a>Stort antal oordnade händelser 

Ett stort antal oordnade händelser inom ett stort tidsfönster gör hello hello ”ordna om bufferten” toobe större storlek. toomitigate den här situationen kan skala hello frågan genom att öka partitioner genom att använda PARTITION. När hello frågan är partitionerad den som är utspridda över flera noder. Därför minskas hello antalet händelser som kommer till varje nod, vilket minskar i sin tur hello storleken på hello beställning buffert. 


## <a name="get-help"></a>Få hjälp
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language-referens](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)
