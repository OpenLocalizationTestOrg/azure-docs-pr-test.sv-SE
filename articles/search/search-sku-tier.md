---
title: "aaaChoose en SKU eller prisnivån för Azure Search | Microsoft Docs"
description: "Azure Search kan etableras på dessa SKU: er: ledigt, Basic eller Standard, där Standard finns i olika resurskonfigurationer och kapacitet för nivåerna."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 8d4b7bca-02a5-43ee-b3f8-03551dfb32fd
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/24/2016
ms.author: heidist
ms.openlocfilehash: eac9a09266db9da4900766808be3bc3b3ff11519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Välj en SKU eller prisnivå för Azure Search
I Azure Search en [service etableras](search-create-service-portal.md) på en specifik prisnivån eller SKU: N. Alternativen är **lediga**, **grundläggande**, eller **Standard**, där **Standard** är tillgänglig i flera konfigurationer och kapacitet.

hello syftet med den här artikeln är toohelp som du väljer en nivå. Om en nivåkapaciteten visar sig toobe för lågt, du behöver tooprovision en ny tjänst på högre nivå för hello och sedan återaktivera ditt index. Det finns ingen uppgradering på plats av hello samma tjänst från en SKU-tooanother.

> [!NOTE]
> När du har valt en nivå och [etablera en söktjänst](search-create-service-portal.md), kan du öka replik och partitionen räknar inom hello-tjänsten. Anvisningar finns [skala resursen nivåer för fråga och indexering arbetsbelastningar](search-capacity-planning.md).
>
>

## <a name="how-tooapproach-a-pricing-tier-decision"></a>Hur tooapproach en prisnivå tjänstnivån beslut
I Azure Search avgör hello nivå kapacitet, inte funktionstillgänglighet. I allmänhet är funktioner tillgängliga på varje nivå, inklusive funktioner för förhandsgranskning. ett undantag för hello stöds inte för indexerare i S3 HD.

> [!TIP]
> Vi rekommenderar att du alltid etablera en **lediga** service (en per prenumeration utan slutdatum) så att dess är tillgängligt för inaktiverad projekt. Använd hello **lediga** för testning och utvärdering; skapar en andra fakturerbar tjänst på hello **grundläggande** eller **Standard** nivå för produktion eller större arbetsbelastningar för testning.
>
>

Gå hand i hand kapacitet och kostnader för hello-tjänsten körs. Informationen i den här artikeln hjälper dig att bestämma vilka SKU levererar hello rätt balans, men för något av den toobe användbart, behöver du minst grov uppskattning hello följande:

* Antal och storlek på index du planerar toocreate
* Antalet och storleken på dokument tooupload
* Vissa uppfattning om frågan volym, vad gäller frågor Per andra (QPS)

Antal och storlek är viktiga eftersom den maximala gränsen nås via en hård gräns på hello antalet index eller dokument i en tjänst eller på resurser (lagring eller repliker) som används av hello-tjänsten. hello faktiska gränsen för tjänsten är beroende på vilket som hämtar används först: resurser eller objekt.

Beräkningar i hand ska hello följande förenkla hello processen:

* **Steg 1** granska hello SKU beskrivningar under toolearn om tillgängliga alternativ.
* **Steg 2** besvara hello frågor under tooarrive på en preliminär beslut.
* **Steg 3** fattar ditt beslut genom att granska hårda gränser för lagring och priser.

## <a name="sku-descriptions"></a>Beskrivningar av SKU
hello följande tabell innehåller beskrivningar av varje nivå.

| Nivå | Primära scenarier |
| --- | --- |
| **Ledigt** |En delad tjänst utan kostnad, används för utvärdering, undersökningen eller små arbetsbelastningar. Frågan genomflöde och indexering varierar beroende på vem mer använder hello-tjänsten eftersom den delas med andra prenumeranter. Kapacitet är liten (50 MB eller 3 index med upp 10 000 dokument varje). |
| **Basic** |Liten produktionsarbetsbelastningar på dedikerad maskinvara. Hög tillgänglighet. Kapaciteten är too3 repliker och 1 partition (2 GB). |
| **S1** |Standard 1 stöder flexibel kombinationer av partitioner (12) och repliker (12) som används för medelstora produktionsarbetsbelastningar på dedikerad maskinvara. Du kan allokera partitioner och repliker i kombinationer som stöds av ett maximalt antal 36 fakturerbar search-enheter. Partitioner är 25 GB och QPS är ungefär 15 frågor per sekund på den här nivån. |
| **S2** |Standard 2 kör större produktionsarbetsbelastningar med hello samma 36 search-enheter som S1 men med större storlek partitioner och repliker. Partitioner är 100 GB och QPS är ungefär 60 frågor per sekund på den här nivån. |
| **S3** |Standard 3 körs proportionellt större produktionsarbetsbelastningar på högre kompletta system i konfigurationer av too12 partitioner eller 12 repliker under 36 search-enheter. Partitioner är 200 GB och QPS är mer än 60 frågor per sekund på den här nivån. |
| **S3 HD** |Standard 3 hög densitet är avsedd för ett stort antal mindre index. Du kan ha upp too3 partitioner på 200 GB. QPS är mer än 60 frågor per sekund. |

> [!NOTE]
> Replik och partitionen maxkapacitet debiteras ut som search-enheter (36 unit maximala per tjänst), vilket medför en effektiv nedre gräns än vad hello maximalt innebär på framsidan värde. Till exempel toouse hello maximalt 12 repliker, du kan ha högst 3 partitioner (12 * 3 = 36 enheter). På liknande sätt minska toouse maximala partitioner, repliker too3. Se [skala resursen nivåer för fråga och indexering arbetsbelastningar i Azure Search](search-capacity-planning.md) för diagram på tillåtna kombinationer.
>
>

## <a name="review-limits-per-tier"></a>Granska gränser för nivån
hello diagrammet nedan är en delmängd av hello gränser från [Tjänstbegränsningarna i Azure Search](search-limits-quotas-capacity.md). Den visar hello faktorer troligen tooimpact SKU beslut. Du kan se toothis diagrammet när du granskar hello frågorna nedan.

| Resurs | Kostnadsfri | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Serviceavtal (SLA) |Nr <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Index gränser |3 |5 |50 |200 |200 |1000 <sup>2</sup> |
| Dokumentet gränser |10 000 totalt |1 miljon per tjänst |15 miljoner per partition |60 miljoner per partition |120 miljoner per partition |1 miljon per index |
| Maximal partitioner |Saknas |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitionsstorlek |Totalt antal 50 MB |2 GB per tjänst |25 GB per partition |100 GB per partition (upp tooa högst 1,2 TB per tjänst) |200 GB per partition (upp tooa högst 2,4 TB per tjänst) |200 GB (upp tooa högst 600 GB per tjänst) |
| Maximal repliker |Saknas |3 |12 |12 |12 |12 |
| Frågor per sekund |Saknas |~ 3 per replik |~ 15 per replik |~ 60 per replik |>60 per replik |>60 per replik |

<sup>1</sup> ledigt nivå och förhandsgranska funktioner inte kommer med servicenivåavtal (SLA). För alla fakturerbar nivåer börjar SLA gälla när du etablerar tillräcklig redundans för din tjänst. Två eller flera repliker krävs för SERVICENIVÅAVTAL för frågan (läsa). Tre eller flera repliker krävs för fråga och indexering SLA (skrivskyddad). hello antalet partitioner är inte ett SLA-faktor. 

<sup>2</sup> S3 och S3 HD backas upp av identiska hög kapacitet infrastruktur, men var och en når sin maximala gränsen på olika sätt. S3 riktar sig till ett mindre antal mycket stora index. Därför dess maxgränsen är bundet till en resurs (2,4 TB för varje tjänst). S3 HD riktar sig till ett stort antal mycket små index. S3 HD når gränsen i hello form av indexet begränsningar på 1 000 index. Om du är en S3 HD kund som kräver mer än 1 000 index, kontaktar du Microsoft Support för mer information om hur tooproceed.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Eliminera SKU: er som inte uppfyller kraven
hello följande frågor kan hjälpa dig komma fram till hello rätt SKU beslut för din arbetsbelastning.

1. Har du **serviceavtalet (SLA)** krav? Du kan använda alla fakturerbar nivå (Basic på dig), men du måste konfigurera din tjänst för redundans. Två eller flera repliker krävs för SERVICENIVÅAVTAL för frågan (läsa). Tre eller flera repliker krävs för fråga och indexering SLA (skrivskyddad). hello antalet partitioner är inte ett SLA-faktor.
2. **Hur många indexerar** behöver du? En av hello största variabler factoring i beslut SKU är hello antal index som stöds av varje SKU. Stöd för index på markant olika nivåer i hello lägre prisnivåer. Krav för antalet index kan vara en avgörande faktorn vad gäller SKU beslut.
3. **Hur många dokument** läses in i varje index? hello antalet och storleken på dokument avgör hello slutliga storleken för hello index. Under förutsättning att du kan beräkna Hej planerade storlek på hello index, kan du jämföra det numret mot hello partitionsstorlek per SKU och utökas med hello antalet partitioner krävs toostore ett index över storleken.
4. **Vad är hello förväntade frågebelastningen**? När lagringskraven förstås, Överväg frågan arbetsbelastningar. S2 och båda S3 SKU: er erbjuder nära motsvarande dataflöde, men SLA-krav ska utesluta alla preview SKU: er.
5. Om du funderar hello S2 eller S3 nivåer avgöra om du behöver [indexerare](search-indexer-overview.md). Indexerare är ännu inte tillgängliga för hello S3 HD nivå. Alternativ metod är toouse en push-modell för index uppdateringar, där du kan skriva program koden toopush en datauppsättning tooan index.

De flesta kunder kan regeln en specifik SKU in eller ut baserat på deras svar toohello ovan frågor. Om du fortfarande inte vet vilka SKU toogo med, du bokför frågor tooMSDN eller StackOverflow forum eller kontakta Azure Support för ytterligare information.

## <a name="decision-validation-does-hello-sku-offer-sufficient-storage-and-qps"></a>Decision validering: erbjuder hello SKU tillräckligt med lagringsutrymme och QPS?
Som ett sista steg besöker hello [sida med priser](https://azure.microsoft.com/pricing/details/search/) och hello [per service och per index avsnitt i Tjänstbegränsningarna](search-limits-quotas-capacity.md) toodouble Kontrollera uppskattningar mot prenumeration och tjänsten gränser.

Om antingen hello pris eller lagring kraven är utanför intervallet, kanske du vill toorefactor hello arbetsbelastningar bland flera mindre tjänster (till exempel). Du kan ändra designen index toobe mindre på detaljnivå, eller Använd filtrerar toomake frågor effektivare.

> [!NOTE]
> Lagringskraven kan vara felaktigt luftfyllda om dokument innehåller främmande data. Vi rekommenderar innehåller dokument bara sökbara data eller metadata. Binära data är icke-sökbart och ska lagras separat (kanske i ett Azure table eller blob storage) med ett fält i hello index toohold en URL toohello externa referensdata. hello maximal storlek för ett enskilt dokument är 16 MB (eller mindre om du är bulk överför flera dokument i en begäran). Se [gränser i Azure Search](search-limits-quotas-capacity.md) för mer information.
>
>

## <a name="next-step"></a>Nästa steg
När du vet vilken SKU är hello höger passar fortsätta på med de här stegen:

* [Skapa en söktjänst i hello-portalen](search-create-service-portal.md)
* [Ändra hello fördelning av partitioner och repliker tooscale din tjänst](search-capacity-planning.md)
