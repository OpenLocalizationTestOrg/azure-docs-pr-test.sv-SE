---
title: "aaaService gränser i Azure Search | Microsoft Docs"
description: "Tjänstbegränsningarna som används för kapacitetsplanering och övre gräns för begäranden och -svar för Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 857a8606-c1bf-48f1-8758-8032bbe220ad
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/07/2017
ms.author: heidist
ms.openlocfilehash: cb13a0f1c87a654fb5845c9c741f74a91da5b372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-limits-in-azure-search"></a>Tjänstbegränsningarna i Azure Search
Gränsvärdet på lagring, arbetsbelastningar och mängder index, dokument och andra objekt är beroende av om du [etablera Azure Search](search-create-service-portal.md) på en **lediga**, **grundläggande**, eller **Standard** prisnivån.

* **Ledigt** är en delad tjänst för flera innehavare som medföljer din Azure-prenumeration. Det är ett alternativ för befintliga prenumeranter utan ytterligare kostnad som du kan använda tooexperiment med hello-tjänsten innan du registrerar dig för dedicerade resurser.
* **Grundläggande** ger dedikerade datorresurser för produktionsarbetsbelastningar i mindre skala.
* **Standard** körs på dedikerade datorer med mer bearbetning och lagring kapacitet på varje nivå. Standard kommer i fyra nivåer: S1, S2, S3 och S3 hög densitet (S3 HD).

> [!NOTE]
> En tjänst har etablerats på en specifik nivå. Om du behöver toojump nivåer tooget högre kapacitet, måste du etablera en ny tjänst (det finns ingen uppgradering på plats). Mer information finns i [Välj en SKU- eller nivå](search-sku-tier.md). Mer information om hur du anpassar inom en tjänst toolearn du har redan etablerats, se [skala resursen nivåer för fråga och indexering arbetsbelastningar](search-capacity-planning.md).
>

## <a name="per-subscription-limits"></a>Per prenumerationsbegränsningar
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="per-service-limits"></a>Per tjänstbegränsningarna
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## <a name="per-index-limits"></a>Per index gränser
Det finns en motsvarande mellan index begränsningar och gränser indexerare. Får högst 200 index hello maxgränsen för indexerare är också 200 för hello samma tjänst.

| Resurs | Kostnadsfri | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Index: maximala fält per index |1000 |100 <sup>1</sup> |1000 |1000 |1000 |1000 |
| Index: maximalt bedömningen profiler per index |100 |100 |100 |100 |100 |100 |
| Index: maximala funktioner per profil |8 |8 |8 |8 |8 |8 |
| Indexerare: största indexering belastning per anrop |10 000 dokument |Begränsas bara av maximum dokument |Begränsas bara av maximum dokument |Begränsas bara av maximum dokument |Begränsas bara av maximum dokument |EJ TILLÄMPLIGT <sup>2</sup> |
| Indexerare: maximala körtiden | 1-3 minuter <sup>3</sup> |24 timmar |24 timmar |24 timmar |24 timmar |EJ TILLÄMPLIGT <sup>2</sup> |
| BLOB-indexeraren: maximala blob, storlek i MB |16 |16 |128 |256 |256 |EJ TILLÄMPLIGT <sup>2</sup> |
| BLOB-indexeraren: maximala antalet tecken innehåll extraheras från ett blob |32,000 |64,000 |4 miljoner |4 miljoner |4 miljoner |EJ TILLÄMPLIGT <sup>2</sup> |

<sup>1</sup> grundnivån är hello endast SKU: N med en nedre gräns på 100 fält per index.

<sup>2</sup> S3 HD inte stöder indexerare. Kontakta Azure-supporten om du har angeläget för den här funktionen.

<sup>3</sup> indexeraren maximala körningstiden för hello kostnadsfria nivån är 3 minuter för blobbkällorna och 1 minut för alla andra datakällor.

## <a name="document-size-limits"></a>Storleksgränser för dokumentet
| Resurs | Kostnadsfri | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Dokumentstorlek för enskilda per Index API |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |< 16 MB |

Refererar toohello maximala storlek när du anropar en API-Index. Storlek är faktiskt en gräns på hello storlek hello Index API frågans brödtext. Eftersom du kan skicka en grupp med flera dokument toohello Index API på samma gång, beroende hello storleksgränsen faktiskt av hur många dokument som finns i hello batch. Hello maximala storlek är 16 MB JSON för en grupp med ett enskilt dokument.

tookeep storlek ned, Kom ihåg tooexclude-frågbar data från hello-begäran. Bilder och andra binära data är inte direkt frågbar och får inte lagras i hello index. toointegrate-frågbar data till sökresultat, definiera ett icke-sökbart fält som lagrar toohello för URL: en referens.

## <a name="workload-limits-queries-per-second"></a>Arbetsbelastningen begränsar (frågor per sekund)
| Resurs | Kostnadsfri | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| QPS |Saknas |~ 3 per replik |~ 15 per replik |~ 60 per replik |>60 per replik |>60 per replik |

Frågor per sekund (QPS) är en uppskattning baserat på heuristik, med hjälp av simulerade och faktiska kunden arbetsbelastningar tooderive beräknade värden. Den exakta QPS genomströmning varierar beroende på dina data och hello uppbyggnad hello frågan.

Även om grov uppskattning tillhandahålls ovan är en verklig frekvens svårt toodetermine, särskilt i hello lediga delad tjänst där genomströmning baserat på tillgängliga bandbredd och konkurrens om systemresurser. I hello kostnadsfria nivån, beräkning och lagring resurser delas av flera prenumeranter så QPS för lösningen alltid kan variera beroende på hur många andra arbetsbelastningar som körs vid hello samtidigt.

Nivån hello standard, kan du uppskatta QPS mer noggrant eftersom du har kontroll över flera hello parametrar. I avsnittet hello bästa praxis i [hantera din sökning lösning](search-manage.md) vägledning om hur toocalculate QPS för din arbetsbelastning.

## <a name="api-request-limits"></a>API-begärandebegränsningar
* Högst 16 MB per begäran <sup>1</sup>
* Maximal URL-längd 8 KB
* Maximal 1000 dokument per batch i indexet överför, sammanfogas eller tar bort
* Maximalt 32 fälten i satsen $orderby
* Maximal Sök är termen 32 766 byte (32 KB minus 2 byte) för UTF-8-kodat text

<sup>1</sup> i Azure Search hello brödtexten i begäran är ämne tooan övre gräns på 16 MB, införa en praktisk gräns på hello innehållet i enskilda fält och samlingar som inte annars är begränsad av teoretisk gränser (se [stöds datatyper](https://msdn.microsoft.com/library/azure/dn798938.aspx) mer information om fältet sammansättning och begränsningar).

## <a name="api-response-limits"></a>API-svar gränser
* Maximal 1000 dokument som returneras per sida i sökresultat
* Maximalt 100 förslag som returneras per föreslår API-begäran

## <a name="api-key-limits"></a>API-nyckel gränser
API-nycklar används för autentiseringen av tjänsten. Det finns två typer. Admin nycklar anges i huvudet i begäran är hello och ge fullständig läs-/ skrivåtkomst toohello service. Frågan nycklar är skrivskyddad, anges på hello URL och tooclient vanligtvis distribuerade program.

* Högst 2 admin nycklar per tjänst
* Högst 50 frågan nycklar per tjänst
