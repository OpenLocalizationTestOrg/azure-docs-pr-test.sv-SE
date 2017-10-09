---
title: "aaaCreate Azure Search index | Microsoft Azure | Söktjänsten värdbaserade moln"
description: "Vad är ett index i Azure Search och hur används det?"
services: search
documentationcenter: 
author: ashmaka
ms.assetid: a395e166-bf2e-4fca-8bfc-116a46c5f7b1
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: c01cc654ff91427c8f1569b2f5b060a0a0f044c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index"></a>Skapa ett Azure Search-index
> [!div class="op_single_selector"]
> * [Översikt](search-what-is-an-index.md)
> * [Portalen](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

## <a name="what-is-an-index"></a>Vad är ett index?
Ett *index* är ett  beständigt arkiv med *dokument* och andra konstruktioner som används av en Azure Search-tjänst. Ett dokument är en enskild enhet med sökbara data i ditt index. En återförsäljare som sysslar med e-handel kan till exempel ha ett dokument för varje artikel som företaget säljer, och en nyhetsorganisation kan ha ett dokument för varje nyhetsartikel osv. Mappa dessa begrepp toomore bekant databasen motsvarigheter: en *index* är begreppsmässigt tooa *tabell*, och *dokument* är ungefär för*rader* i en tabell.

När du lägger till/Överför dokument och skicka Sök frågor tooAzure sökning, skicka ditt begäranden tooa index i din söktjänst.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Fälttyper och fältattribut i ett Azure Search-index
När du definierar schemat måste du ange hello namn, typ och attribut för varje fält i ditt index. hello fälttypen klassificerar hello data som lagras i fältet. Attribut kan anges på enskilda fält toospecify hur hello fältet används. hello följande tabeller räkna upp hello typer och attribut som du kan ange.

### <a name="field-types"></a>Fälttyper
| Typ | Beskrivning |
| --- | --- |
| *Edm.String* |Text som kan tokeniseras för textsökning (radbrytning, ordstamsigenkänning osv). |
| *Collection(Edm.String)* |En lista med strängar som kan tokeniseras för textsökning. Det finns ingen teoretisk övre gräns för hello antalet objekt i en samling, men hello 16 MB övre gräns på nyttolastens storlek gäller toocollections. |
| *Edm.Boolean* |Innehåller sant/falskt-värden. |
| *Edm.Int32* |32-bitars heltalsvärden. |
| *Edm.Int64* |64-bitars heltalsvärden. |
| *Edm.Double* |Numeriska data med dubbel precision. |
| *Edm.DateTimeOffset* |Datum tidsvärden med hello OData V4-format (t.ex. `yyyy-MM-ddTHH:mm:ss.fffZ` eller `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |En plats som representerar en geografisk plats på hello jordglob. |

Mer detaljerad information om [vilka datatyper som stöds i Azure Search finns här](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types).

### <a name="field-attributes"></a>Fältattribut
| Attribut | Beskrivning |
| --- | --- |
| *Nyckel* |En sträng som innehåller hello unikt ID för varje dokument som används för dokumentet slå upp. Alla index måste ha en nyckel. Bara ett fält kan vara hello nyckel och dess typ måste anges tooEdm.String. |
| *Hämtningsbar* |Anger om ett fält kan returneras i sökresultat. |
| *Filtrerbar* |Tillåter hello fältet toobe används i filterfrågor. |
| *Sorterbar* |Gör en fråga toosort sökresultat med hjälp av det här fältet. |
| *Fasettbar* |Gör en fältet toobe som används i en [fasetterad navigering](search-faceted-navigation.md) struktur för användaren själv dirigerad filtrering. Vanligtvis fält som innehåller upprepade värden som du kan använda toogroup flera dokument tillsammans (till exempel flera dokument som omfattas av ett enda varumärke eller tjänstekategori) fungerar bäst som facets. |
| *Sökbar* |Markerar hello fält som sökbara fulltext. |

Mer detaljerad information om [indexattributen i Azure Search finns här](https://docs.microsoft.com/rest/api/searchservice/Create-Index).

## <a name="guidance-for-defining-an-index-schema"></a>Riktlinjer för att definiera ett indexschema
Ta din tid i hello planera fasen toothink via varje beslut när du utformar ditt index. Det är viktigt att du behåller dina Sök upplevelse och företag behov i åtanke när du skapar ditt index som varje fält måste tilldelas hello [rätt attribut](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Ändra ett index när den har distribuerats innebär att återskapa och ladda om hello informationen.

Om datalagringsbehovet ändras med tiden kan du öka eller minska kapaciteten genom att lägga till eller ta bort partitioner. Mer information finns i [Hantera din söktjänst i Azure](search-manage.md) och [Tjänstgränser](search-limits-quotas-capacity.md).

