---
title: "Kapacitetsplanering för Azure Search | Microsoft Docs"
description: "Justera partition och repliken datorresurser i Azure Search, där varje resurs prissätts i fakturerbar search-enheter."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 1dc16afe-56f9-439d-8874-1733ae1a2b74
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 02/08/2017
ms.author: heidist
ms.openlocfilehash: 26f5e71f3d00161a92de702209e224008ec8a5ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>Skala resursen nivåer för fråga och indexering arbetsbelastningar i Azure Search
När du [Välj prisnivå](search-sku-tier.md) och [etablera en söktjänst](search-create-service-portal.md), nästa steg är att om du vill öka antalet repliker eller partitioner som används av din tjänst. Varje nivå erbjuder ett fast antal fakturering enheter. Den här artikeln beskriver hur du tilldela dessa enheter för att uppnå en optimal konfiguration som balanserar dina krav för körning av fråga, indexering och lagring.

Resurskonfigurationen är tillgänglig när du skapar en tjänst på den [grundläggande nivån](http://aka.ms/azuresearchbasic) eller en av de [Standard nivåer](search-limits-quotas-capacity.md). För fakturerbar services på dessa nivåer kapacitet köps i steg om *sökenheter* (SUs) där varje partition och repliken räknas som en SU. 

Använder färre SUs-resultat i en proportionerligt lägre faktura. Fakturering används för så länge som tjänsten har ställts in. Om du tillfälligt inte använder en tjänst, är det enda sättet att undvika fakturering genom att ta bort tjänsten och sedan återskapa den när du behöver den.

> [!Note]
> Tar bort en tjänst allt på den. Det finns inga anläggning under Azure Search för att säkerhetskopiera och återställa sparade sökningen data. Om du vill distribuera om ett befintligt index på en ny tjänst, bör du köra det program som används för att skapa och läsa in den ursprungligen. 

## <a name="terminology-partitions-and-replicas"></a>Terminologi: partitioner och repliker
Partitioner och repliker är de primära resurser som stöder en söktjänst.

| Resurs | Definition |
|----------|------------|
|*Partitioner* | Innehåller index lagrings- och i/o för läsning/skrivning (t.ex, när återskapa eller uppdatera ett index).|
|*Repliker* | Instanser av söktjänsten används huvudsakligen för att läsa in saldo frågeåtgärder. Varje replik värd alltid en kopia av ett index. Om du har 12 repliker har du 12 kopior av varje index som har lästs in på tjänsten.|

> [!NOTE]
> Det går inte att direkt hantera eller hantera vilka index kör på en replik. En kopia av varje indexet för varje replik är en del av tjänsten-arkitekturen.
>

## <a name="how-to-allocate-partitions-and-replicas"></a>Så här allokerar du partitioner och repliker
I Azure Search allokeras en tjänst först en minimal nivå på resurser som består av en partition och en replik. Du kan inkrementellt justera resurser genom att öka partitioner om du behöver mer lagringsutrymme och i/o, eller lägga till flera repliker för större volymer i frågan eller bättre prestanda för nivåer som stöds. En enskild tjänst måste ha tillräckligt med resurser för att hantera alla arbetsbelastningar (indexering och frågor). Du kan inte dela upp arbetsbelastningar bland flera tjänster.

Om du vill öka eller ändra fördelningen av repliker och partitioner, bör du använda Azure-portalen. Portalen framtvingar begränsningar för tillåtna kombinationer som vara under gränsvärden:

1. Logga in på den [Azure-portalen](https://portal.azure.com/) och välj search-tjänsten.
2. I **inställningar**öppnar den **skala** bladet och Använd skjutreglagen för att öka eller minska antalet partitioner och repliker.

Om du behöver ett skript eller kod etablering metoden den [Management REST API](https://msdn.microsoft.com/library/azure/dn832687.aspx) är ett alternativ till portalen.

I allmänhet behöver sökprogram mer repliker än partitioner, särskilt när tjänståtgärderna är prioriterar frågan arbetsbelastningar. Avsnittet [hög tillgänglighet](#HA) förklarar varför.

> [!NOTE]
> När en tjänst har etablerats kan inte uppgraderas till en högre SKU. Du behöver skapa en söktjänst på ny nivå och Läs in ditt index. Se [skapa en Azure Search-tjänst i portalen](search-create-service-portal.md) hjälp med service etablering.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Hög tillgänglighet
Eftersom det är enkelt och relativt snabbt att skala upp, vanligtvis rekommenderas att du börjar med en partition och en eller två repliker och sedan skalas upp som frågan volymer skapa. En partition innehåller för många services på nivåerna Basic eller S1 tillräcklig lagring och i/o (på 15 miljoner dokument per partition).

Frågan arbetsbelastningar som körs i första hand på repliker. Om du behöver mer genomflöde och hög tillgänglighet kan kräver du förmodligen ytterligare repliker.

Allmänna rekommendationer för hög tillgänglighet är:

* Två repliker för hög tillgänglighet för skrivskyddade arbetsbelastningar (frågor)
* Tre eller flera repliker för hög tillgänglighet för läsning och skrivning arbetsbelastningar (frågor plus indexering när enskilda dokument läggs till, uppdatera eller ta bort)

Servicenivåavtal (SLA) för Azure Search är inriktade på frågor och uppdateringar av index som består av att lägga till, uppdatera eller ta bort dokument.

### <a name="index-availability-during-a-rebuild"></a>Index tillgänglighet under en återskapning

Hög tillgänglighet för Azure Search gäller frågor och index uppdateringar som inte rör när ett index. Om du tar bort ett fält, ändra datatypen eller Byt namn på ett fält måste återskapa indexet. Om du vill återskapa indexet, måste du ta bort indexet, återskapa indexet och Läs in data.

> [!NOTE]
> Du kan lägga till nya fält till ett Azure Search-index utan att återskapa indexet. Värdet för det nya fältet ska vara null för alla dokument redan i indexet.

Om du vill behålla index tillgänglighet under en återskapning, måste du ha en kopia av indexet med ett annat namn på samma tjänst eller en kopia av indexet med samma namn på en annan tjänst och ange sedan omdirigering eller redundanslogiken i koden.

## <a name="disaster-recovery"></a>Haveriberedskap
Det finns för närvarande ingen inbyggd mekanism för katastrofåterställning. Lägga till partitioner eller repliker är fel strategin för att uppfylla disaster recovery mål. Det vanligaste sättet är att lägga till redundans på tjänstnivå genom att skapa en andra söktjänsten i en annan region. Precis som med tillgänglighet under ett index rebuild måste omdirigering eller redundanslogiken komma från din kod.

## <a name="increase-query-performance-with-replicas"></a>Öka prestanda för frågor med repliker
Svarstid är en indikator att ytterligare repliker behövs. I allmänhet är ett första steg mot att förbättra frågeprestanda att lägga till mer av den här resursen. När du lägger till repliker fler kopior av indexet åter är online för att stödja större belastningar i fråga och läsa in Utjämna begäranden över flera repliker.

Vi kan inte tillhandahålla hårda beräknar på frågor per sekund (QPS): frågan prestanda beror på komplexiteten i frågan och konkurrerande arbetsbelastningar. I genomsnitt en replik på grundläggande eller S1 SKU: er kan betjäna ungefär 15 QPS, men din genomströmning ska vara högre eller lägre beroende på komplexiteten i frågan (fasetterad frågor är mer komplexa) och nätverkssvarstid. Det är också viktigt att känna igen att även om repliker definitivt lägger läggs skalning och prestanda, resultatet inte är absolut linjär: lägga till tre repliker garanterar inte tre genomflöde.

Läs mer om QPS, inklusive metoder för att bedöma QPS för din arbetsbelastning i [hantera din söktjänst](search-manage.md).

## <a name="increase-indexing-performance-with-partitions"></a>Öka indexering prestanda med partitioner
Sökprogram som kräver nära realtid datauppdatering behöver proportionerligt fler partitioner än repliker. Lägga till partitioner sprids läs-/ skrivåtgärder över fler beräkningsresurser. Det ger dig också mer diskutrymme för att lagra ytterligare index och dokument.

Större index ta längre tid att fråga. Därför kanske du upptäcker att varje stegvis ökning partitioner kräver en mindre men proportionell ökning av replikerna. Komplexitet på dina frågor och fråga volym kommer factor i hur snabbt Frågekörningen är aktiverat.

## <a name="basic-tier-partition-and-replica-combinations"></a>Grundnivån: kombinationer av Partition och replikering
En grundläggande tjänst kan ha exakt en partition och upp till tre repliker för maximalt begränsa tre SUS. Endast justerbara resursen är repliker. Du behöver minst två repliker för hög tillgänglighet för frågor.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standard nivåer: kombinationer av Partition och replikering
Den här tabellen visar SUs som krävs för att stödja kombinationer av repliker och partitioner omfattas 36 SU-gränsen för alla nivåer som Standard.

|   | **1 partition** | **2 partitioner** | **3 partitioner** | **4 partitioner** | **6 partitioner** | **12 partitioner** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 replik** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 repliker** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 repliker** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 repliker** |4 SU |8 SU |12 SU |16 SU |24 SU |Saknas |
| **5 repliker** |5 SU |10 SU |15 SU |20 SU |30 SU |Saknas |
| **6 repliker** |6 SU |12 SU |18 SU |24 SU |36 SU |Saknas |
| **12 repliker** |12 SU |24 SU |36 SU |Saknas |Saknas |Saknas |

SUs priser och kapacitet beskrivs i detalj på Azure-webbplatsen. Mer information finns i [prisinformation](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Antal repliker och partitioner som delar upp jämnt i 12 (särskilt 1, 2, 3, 4, 6, 12). Det beror på att Azure Search delar före varje index i 12 shards så att den kan spridas lika stora delar för alla partitioner. Om din tjänst har tre partitioner och du skapar ett index, till exempel innehåller varje partition fyra delar av indexet. Hur Azure Search delar ett index är en implementering detaljer, utgåvor kan ändras i framtida. Även om talet är 12 idag, bör inte kommer detta antal ska alltid vara 12 i framtiden.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>Fakturering formeln för replik och partitionen resurser
Formel för att beräkna hur många SUs används för särskilda kombinationer är produkt i repliker och partitioner, eller (R X P = SU). Till exempel faktureras tre repliker multiplicerat med tre partitioner som nio SUs.

Kostnaden per SU bestäms av nivån med en lägre per enhet fakturering hastighet för grundläggande än för Standard. Bedömer för varje nivå kan hittas på [prisinformation](https://azure.microsoft.com/pricing/details/search/).
