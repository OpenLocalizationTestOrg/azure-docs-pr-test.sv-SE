---
title: "Planera för Azure Search aaaCapacity | Microsoft Docs"
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
ms.openlocfilehash: 4bbbb929a36b932ea7af12e494ca095d98b9005e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>Skala resursen nivåer för fråga och indexering arbetsbelastningar i Azure Search
När du [Välj prisnivå](search-sku-tier.md) och [etablera en söktjänst](search-create-service-portal.md), hello nästa steg är toooptionally öka hello antal repliker eller partitioner som används av din tjänst. Varje nivå erbjuder ett fast antal fakturering enheter. Den här artikeln förklarar hur tooallocate dessa enheter tooachieve en optimal konfiguration som balanserar dina krav för körning av fråga, indexering och lagring.

Resurskonfigurationen är tillgänglig när du skapar en tjänst på hello [grundläggande nivån](http://aka.ms/azuresearchbasic) eller en av hello [Standard nivåer](search-limits-quotas-capacity.md). För fakturerbar services på dessa nivåer kapacitet köps i steg om *sökenheter* (SUs) där varje partition och repliken räknas som en SU. 

Använder färre SUs-resultat i en proportionerligt lägre faktura. Fakturering används för så länge hello-tjänsten har konfigurerats. Om du tillfälligt inte använder en tjänst, hello endast sätt tooavoid faktureringen är genom att ta bort hello-tjänsten och sedan återskapa den när du behöver den.

> [!Note]
> Tar bort en tjänst allt på den. Det finns inga anläggning under Azure Search för att säkerhetskopiera och återställa sparade sökningen data. tooredeploy ett befintligt index på en ny tjänst kör hello används toocreate och Läs in den ursprungligen. 

## <a name="terminology-partitions-and-replicas"></a>Terminologi: partitioner och repliker
Partitioner och repliker är hello primära resurser som stöder en söktjänst.

| Resurs | Definition |
|----------|------------|
|*Partitioner* | Innehåller index lagrings- och i/o för läsning/skrivning (t.ex, när återskapa eller uppdatera ett index).|
|*Repliker* | Instanser av hello söktjänsten används främst tooload saldo frågeåtgärder. Varje replik värd alltid en kopia av ett index. Om du har 12 repliker har du 12 kopior av varje index som har lästs in på hello-tjänsten.|

> [!NOTE]
> Det finns inget sätt toodirectly manipulera eller hantera vilka index kör på en replik. En kopia av varje indexet för varje replik är en del av hello service-arkitektur.
>

## <a name="how-tooallocate-partitions-and-replicas"></a>Hur tooallocate partitioner och repliker
I Azure Search allokeras en tjänst först en minimal nivå på resurser som består av en partition och en replik. Du kan inkrementellt justera resurser genom att öka partitioner om du behöver mer lagringsutrymme och i/o, eller lägga till flera repliker för större volymer i frågan eller bättre prestanda för nivåer som stöds. En enskild tjänst måste ha tillräckliga resurser toohandle alla arbetsbelastningar (indexering och frågor). Du kan inte dela upp arbetsbelastningar bland flera tjänster.

tooincrease eller ändra hello allokering av repliker och partitioner, bör du använda hello Azure-portalen. hello portal framtvingar begränsningar för tillåtna kombinationer som vara under gränsvärden:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) och välj hello search-tjänsten.
2. I **inställningar**öppnar hello **skala** bladet och använda Hej reglage tooincrease eller minska hello antalet partitioner och repliker.

Om du behöver ett skript eller kod etablering tillvägagångssätt hello [Management REST API](https://msdn.microsoft.com/library/azure/dn832687.aspx) är en alternativ toohello portal.

I allmänhet behöver sökprogram mer repliker än partitioner, särskilt när hello tjänståtgärder är prioriterar frågan arbetsbelastningar. Hej avsnitt på [hög tillgänglighet](#HA) förklarar varför.

> [!NOTE]
> När en tjänst har etablerats kan den inte uppgraderade tooa högre SKU. Du behöver toocreate search-tjänsten på hello ny nivå och Läs in ditt index. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) hjälp med service etablering.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Hög tillgänglighet
Eftersom det är enkelt och relativt snabbt tooscale in vanligtvis rekommenderas att du börjar med en partition och en eller två repliker och sedan skalas upp som frågan volymer skapa. En partition innehåller för många services på hello Basic eller S1 nivåer tillräcklig lagring och i/o (på 15 miljoner dokument per partition).

Frågan arbetsbelastningar som körs i första hand på repliker. Om du behöver mer genomflöde och hög tillgänglighet kan kräver du förmodligen ytterligare repliker.

Allmänna rekommendationer för hög tillgänglighet är:

* Två repliker för hög tillgänglighet för skrivskyddade arbetsbelastningar (frågor)
* Tre eller flera repliker för hög tillgänglighet för läsning och skrivning arbetsbelastningar (frågor plus indexering när enskilda dokument läggs till, uppdatera eller ta bort)

Servicenivåavtal (SLA) för Azure Search är inriktade på frågor och uppdateringar av index som består av att lägga till, uppdatera eller ta bort dokument.

### <a name="index-availability-during-a-rebuild"></a>Index tillgänglighet under en återskapning

Hög tillgänglighet för Azure Search gäller tooqueries och index uppdateringar som inte rör när ett index. Om du tar bort ett fält, ändra datatypen eller Byt namn på ett fält måste toorebuild hello index. toorebuild hello index, måste du ta bort hello index, återskapa hello index och Läs in hello data.

> [!NOTE]
> Du kan lägga till nya fält tooan Azure Search index utan att återskapa hello indexet. hello värdet för hello nya fältet ska vara null för alla dokument redan i hello index.

toomaintain index tillgänglighet under en återskapning, du måste ha en kopia av hello index med ett annat namn på hello samma tjänst, eller en kopia av hello index med hello samma namn på en annan tjänst och ange sedan omdirigering eller failover logiken i din kod.

## <a name="disaster-recovery"></a>Haveriberedskap
Det finns för närvarande ingen inbyggd mekanism för katastrofåterställning. Lägga till partitioner eller repliker är hello fel strategi för att uppfylla disaster recovery mål. hello vanligaste metoden är tooadd redundans på hello servicenivå genom att skapa en andra söktjänsten i en annan region. Precis som med tillgänglighet under ett index rebuild måste hello omdirigering eller redundanslogiken komma från din kod.

## <a name="increase-query-performance-with-replicas"></a>Öka prestanda för frågor med repliker
Svarstid är en indikator att ytterligare repliker behövs. Ett första steg mot att förbättra frågeprestanda är oftast mer tooadd för den här resursen. När du lägger till repliker fler kopior av hello index förs online toosupport större frågan arbetsbelastningar och tooload saldo hello-begäranden över hello flera repliker.

Vi kan inte tillhandahålla hårda beräknar på frågor per sekund (QPS): frågan prestanda beror på hello komplexitet hello frågan och konkurrerande arbetsbelastningar. I genomsnitt en replik på grundläggande eller S1 SKU: er kan betjäna ungefär 15 QPS, men din genomströmning ska vara högre eller lägre beroende på komplexiteten i frågan (fasetterad frågor är mer komplexa) och nätverkssvarstid. Det är också viktigt toorecognize som även om repliker definitivt lägger läggs skalning och prestanda, hello resultatet inte är absolut linjär: lägga till tre repliker garanterar inte tre genomflöde.

toolearn om QPS, inklusive metoder för att bedöma QPS för din arbetsbelastning finns [hantera din söktjänst](search-manage.md).

## <a name="increase-indexing-performance-with-partitions"></a>Öka indexering prestanda med partitioner
Sökprogram som kräver nära realtid datauppdatering behöver proportionerligt fler partitioner än repliker. Lägga till partitioner sprids läs-/ skrivåtgärder över fler beräkningsresurser. Det ger dig också mer diskutrymme för att lagra ytterligare index och dokument.

Större index ta längre tooquery. Därför kanske du upptäcker att varje stegvis ökning partitioner kräver en mindre men proportionell ökning av replikerna. hello komplexiteten med att frågorna och fråga volym kommer factor i hur snabbt Frågekörningen är aktiverat.

## <a name="basic-tier-partition-and-replica-combinations"></a>Grundnivån: kombinationer av Partition och replikering
En grundläggande tjänst kan ha exakt en partition och upp toothree repliker för högst tre SUs. hello endast justerbara resursen är repliker. Du behöver minst två repliker för hög tillgänglighet för frågor.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standard nivåer: kombinationer av Partition och replikering
Den här tabellen visar hello SUs krävs toosupport kombinationer av repliker och partitioner, ämne toohello 36 SU gränsen för alla nivåer som Standard.

|   | **1 partition** | **2 partitioner** | **3 partitioner** | **4 partitioner** | **6 partitioner** | **12 partitioner** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 replik** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 repliker** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 repliker** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 repliker** |4 SU |8 SU |12 SU |16 SU |24 SU |Saknas |
| **5 repliker** |5 SU |10 SU |15 SU |20 SU |30 SU |Saknas |
| **6 repliker** |6 SU |12 SU |18 SU |24 SU |36 SU |Saknas |
| **12 repliker** |12 SU |24 SU |36 SU |Saknas |Saknas |Saknas |

SUs priser och kapacitet beskrivs i detalj på hello Azure-webbplatsen. Mer information finns i [prisinformation](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> hello antal repliker och partitioner som delar upp jämnt i 12 (särskilt 1, 2, 3, 4, 6, 12). Det beror på att Azure Search delar före varje index i 12 shards så att den kan spridas lika stora delar för alla partitioner. Om din tjänst har tre partitioner och du skapar ett index, till exempel innehåller varje partition fyra delar av hello index. Hur Azure Search delar ett index är en implementering detaljer, utgåvor ämne toochange i framtida. Visserligen hello nummer 12 idag, bör du förväntar dig som number tooalways vara hello 12 framtida.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>Fakturering formeln för replik och partitionen resurser
hello formel för att beräkna hur många SUs används för särskilda kombinationer är hello produkt i repliker och partitioner, eller (R X P = SU). Till exempel faktureras tre repliker multiplicerat med tre partitioner som nio SUs.

Kostnaden per SU bestäms av hello nivån med en lägre per enhet fakturering hastighet för grundläggande än för Standard. Bedömer för varje nivå kan hittas på [prisinformation](https://azure.microsoft.com/pricing/details/search/).
