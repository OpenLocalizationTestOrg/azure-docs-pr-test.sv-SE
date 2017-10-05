---
title: "Azure Stream Analytics: Optimera ditt jobb för att använda enheter för strömning effektivt | Microsoft Docs"
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
ms.openlocfilehash: 1441a5df4fd92abf85763ca9a1512503b1a0da56
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-your-job-to-use-streaming-units-efficiently"></a>Optimera ditt jobb för att effektivt använda enheter för strömning

Azure Stream Analytics aggregerar prestanda ”vikt” för ett jobb som körs i enheter för strömning (SUs). SUs representerar de resurser som används för att köra ett jobb. SU:er är ett sätt att beskriva den relativa kapaciteten för händelsebearbetning utifrån en kombination av mått för processor, minne, och läs- och skrivhastigheter. Den här kapaciteten kan du fokusera på att frågan logik och tar bort du behöver känna lagring tjänstnivån prestandaöverväganden allokera minne för ditt jobb manuellt och ungefärlig core-antalet processorer för att köra ditt jobb inom rimlig tid.

## <a name="how-many-sus-are-required-for-a-job"></a>Hur många SUs krävs för ett jobb?

Att välja antalet nödvändiga SUs för ett visst jobb beror på hur partition för indata och den fråga som definieras i jobbet. Den **skala** bladet kan du ange antalet SUs. Det är en bra idé att allokera mer SUs än vad som behövs. Stream Analytics-bearbetningsmotorn optimeras för svarstid och genomströmning på bekostnad av ytterligare minnesallokering.

I allmänhet är det bästa sättet är att starta med 6 SUs för frågor som inte använder *PARTITION BY*. Sedan fastställa av platsen genom att använda en prövningsmetod med vilket ändra antalet SUs när du skickar representativt datamängder och undersöka SU % användning mått.

Azure Stream Analytics behåller händelser i ett fönster som kallas ”beställning bufferten” innan den börjar eventuell bearbetning. Händelser är sorterade i fönstret beställning av tid och efterföljande åtgärder utförs för närvarande sorterade-händelser. Ordningen händelser vid tidpunkten säkerställer att operatorn har insyn i alla händelser i den angivna tidsperioden. Du kan också operatorn utför nödvändiga bearbetning och ger utdata. En sidoeffekt av denna mekanism är att bearbetningen försenas av tidsperioden för att ändra ordning. Minneskrav för jobbet (vilket påverkar SU förbrukning) är en funktion av storleken på fönstret Ändra ordning och antalet händelser som finns i den.

> [!NOTE]
> När antalet läsare ändras under jobbet uppgraderingar, skrivs tillfälligt varningar till granskningsloggar. Stream Analytics-jobb återskapa automatiskt från dessa tillfälliga problem.

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a>Vanliga orsaker till hög minne för SU hårt för att köra jobb

### <a name="high-cardinality-for-group-by"></a>Hög kardinalitet för GROUP BY

Inkommande händelsers kardinalitet avgör minnesanvändning för jobbet.

Till exempel i `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hur många som är associerade med **klustrade** är kardinalitet för frågan.

För att åtgärda problem som orsakas av hög kardinalitet, skala ut frågan genom att öka partitioner som använder **PARTITION BY**.

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

Antalet *klustrade* är här till Kardinaliteten för GROUP BY.

Efter att frågan är partitionerad den som är utspridda över flera noder. Därför kan minskas antalet händelser som kommer till varje nod, vilket i sin tur minskar storleken på bufferten som beställning. Du bör också partitionera event hub partitioner av partitionid.

### <a name="high-unmatched-event-count-for-join"></a>Hög omatchat antal för koppling

Antalet omatchade händelser i en koppling påverkar frågan minnesanvändning. Till exempel ta en fråga som du vill ta reda på antalet Annonsexponeringar som genererar klick:

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

Det är möjligt att många annonser visas och några klickningar skapas i det här scenariot. Om detta inträffar skulle kräva jobbet att behålla alla händelser inom tidsperioden. Mängden minne som används är proportionell mot fönstret storlek och händelsen hastighet. 

Skala ut frågan genom att öka partitioner genom att använda PARTITION för att minska den här situationen. 

Efter att frågan är partitionerad den som är utspridda över flera noder för bearbetning. Därför kan minskas antalet händelser som kommer till varje nod, vilket i sin tur minskar storleken på bufferten som beställning.

### <a name="large-number-of-out-of-order-events"></a>Stort antal oordnade händelser 

Ett stort antal oordnade händelser inom ett stort tidsfönster orsakar storlek buffertens ”ändra” ska vara större. Skala frågan för att minska den här situationen genom att öka partitioner genom att använda PARTITION. Efter att frågan är partitionerad den som är utspridda över flera noder. Därför kan minskas antalet händelser som kommer till varje nod, vilket i sin tur minskar storleken på bufferten som beställning. 


## <a name="get-help"></a>Få hjälp
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion till Azure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language-referens](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)
