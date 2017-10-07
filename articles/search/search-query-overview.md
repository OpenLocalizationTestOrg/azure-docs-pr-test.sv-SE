---
title: aaaQuery ditt Azure Search Index | Microsoft Docs
description: "Skapa en sökfråga i Azure search och använda Sök parametrar toofilter och sortera sökresultaten."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: ashmaka
ms.openlocfilehash: 4a5ffffe179695fc09446760e21a738dd36c29b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index"></a>Skicka frågor mot ditt Azure Search-index
> [!div class="op_single_selector"]
> * [Översikt](search-query-overview.md)
> * [Portalen](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

När sökningen begäranden tooAzure sökning, finns det ett antal parametrar som kan anges tillsammans med hello faktiska ord som du skriver i sökrutan för hello av ditt program. Dessa frågeparametrar Tillåt tooachieve vissa djupare kontroll över hello [fulltextsökning upplevelse](search-lucene-query-architecture.md).

Nedan visas en lista som kort beskriver vanliga användningsområden för Azure Search hello Frågeparametrar. Fullständig information om frågeparametrar och deras beteende finns hello detaljerad sidor för hello [REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) och [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters#microsoft_azure_search_models_searchparameters#properties_summary).

## <a name="types-of-queries"></a>Typer av frågor
Azure Search erbjuder många alternativ toocreate mycket kraftfulla frågor. hello två typer av frågan som du ska använda är `search` och `filter`. En `search` fråga söker efter ett eller flera villkor i alla *sökbara* fält i ditt index och hello sätt som du kan förvänta dig en sökmotor som Google eller Bing toowork. En `filter`-fråga utvärderar ett booleskt uttryck i alla *filtrerbara* fält i ett index. Till skillnad från `search` frågor, `filter` frågor matchar hello exakt innehållet i ett fält, vilket innebär att de är skiftlägeskänsliga för strängfält.

Du kan använda sökningar och filter tillsammans eller separat. Om du använder dem tillsammans hello filtret är tillämpas första toohello hela indexet och sedan hello sökningen görs på hello filtrets hello resultat. Filter kan därför vara en bra metod tooimprove frågeprestanda eftersom de minska hello uppsättning dokument som hello Sök frågan behov tooprocess.

hello syntax för filteruttryck är en delmängd av hello [OData filter-språket](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search). Du kan använda antingen hello för sökfrågor [förenklad syntax](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) eller hello [Lucene frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) som beskrivs nedan.

### <a name="simple-query-syntax"></a>Enkel frågesyntax
Hej [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) är hello standard frågespråk som används i Azure Search. hello enkla frågesyntaxen stöder ett antal vanliga sökoperatorer inklusive hello AND, OR, inte frasen, suffix och prioritet operatörer.

### <a name="lucene-query-syntax"></a>Lucene-frågesyntaxen
Hej [Lucene frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) gör att du toouse hello brett och lättfattliga frågespråk som utvecklats som en del av [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Om du använder den här frågesyntaxen kan du tooeasily uppnå hello följande funktioner: [fältet omfång frågor](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields), [fuzzy Sök](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy), [närhet Sök](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity), [ term den](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost), [reguljära uttryck](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex), [sökning med jokertecken](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard), [syntax grunderna](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax), och [frågor med booleska operatorer](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean).

## <a name="ordering-results"></a>Ordna resultaten
När du får resultat för en sökfråga, kan du begära att Azure Search fungerar hello resultaten sorteras efter värden i ett visst fält. Som standard sorterar Azure Search hello sökresultat baserat på hello rangordningen för varje dokument Sök poäng, som härleds från [TF IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Om du vill att Azure Search tooreturn resultat sorterade efter ett annat värde än hello Sök poäng, kan du använda hello `orderby` sökparameter. Du kan ange hello värdet för hello `orderby` parametern tooinclude fältet namn och anropar toohello [ `geo.distance()` funktionen](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) för geospatiala värden. Varje uttryck kan följas av `asc` tooindicate att resultat har begärts i stigande ordning, och `desc` tooindicate att resultat har begärts i fallande ordning. hello standard rangordning stigande ordning.

## <a name="paging"></a>Sidindelning
Azure Search gör det enkelt tooimplement växling av sökresultat. Med hjälp av hello `top` och `skip` parametrar, kan du smidigt utfärda search-begäranden som tillåter tooreceive hello totala uppsättning sökresultat i hanterbara, ordnad delmängder som enkelt aktivera bra Sök UI-metoder. Du kan också få hello antalet dokument i hello totala mängden sökresultat när du tar emot dessa mindre delmängder av resultaten.

Du kan lära dig mer om växling sökresultat i hello artikel [hur toopage sökresultat i Azure Search](search-pagination-page-layout.md).

## <a name="hit-highlighting"></a>Träffmarkering
I Azure Search som betonar hello exakt del av sökresultatet som matchar hello sökfråga görs enkelt med hjälp av hello `highlight`, `highlightPreTag`, och `highlightPostTag` parametrar. Du kan ange vilka *sökbara* fält ska ha sina matchade texten framhävs samt att ange hello exakt sträng taggar tooappend toohello början och slutet av hello matchad text Azure sökningen returnerar.

## <a name="try-out-query-syntax"></a>Testa frågesyntax

hello bästa sätt toounderstand skillnader i syntaxen är genom att skicka frågor och granska resultatet.

+ Använd [Sök Explorer](search-explorer.md) i hello Azure-portalen. Genom att distribuera [hello exempel index](search-get-started-portal.md), du kan fråga hello index i minuter med hjälp av verktyg i hello-portalen.

+ Använd [Fiddler](search-fiddler.md) eller att du har överfört tooyour söktjänsten Chrome Postman toosubmit frågor tooan index. Båda verktygen stöder REST-anrop tooan HTTP-slutpunkten. 
