---
title: "aaaHow toopage sökresultat i Azure Search | Microsoft Docs"
description: "Sidbrytning i Azure Search värdbaserade moln search-tjänsten på Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: e3abc1ca4d5994b0a77955379081a4fcfa5a7fa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopage-search-results-in-azure-search"></a>Hur toopage sökresultat i Azure Search
Den här artikeln innehåller råd om hur toouse hello Azure Söktjänsts-REST API tooimplement standardelement en sökning resultatsida, till exempel totalt antal, hämta dokument, sorteringsordningar och navigering.

I samtliga fall som anges nedan, sidrelaterade som bidrar med data eller information tooyour sökresultatsidan anges via hello [Sök efter dokument](http://msdn.microsoft.com/library/azure/dn798927.aspx) förfrågningar som skickas tooyour Azure Search-tjänsten. Begäran innehålla en GET-command, sökväg, och frågeparametrar som information om vad som efterfrågas hello-tjänsten och hur tooformulate hello svar.

> [!NOTE]
> En giltig begäran innehåller ett antal element, till exempel en URL: en och sökväg, HTTP-verb `api-version`och så vidare. Planeringsaspekter bort vi hello exempel toohighlight bara hello syntax som är relevanta toopagination. Se hello [Azure Söktjänsts-REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentationen för mer information om begäran-syntax.
> 
> 

## <a name="total-hits-and-page-counts"></a>Totalt antal träffar och sida räknar
Visar hello Totalt antal resultat har returnerats från en fråga och returnerar de resulterar i mindre segment är de grundläggande toovirtually alla sidor i sökningen.

![][1]

I Azure Search använder du hello `$count`, `$top`, och `$skip` parametrar tooreturn dessa värden. hello följande exempel visas ett exempel på begäran för totala träffar som `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Hämta dokument i grupper med 15, och även visar hello Totalt antal träffar, början på första sidan i hello:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Paginating resultat kräver båda `$top` och `$skip`, där `$top` anger hur många objekt tooreturn i en batch och `$skip` anger hur många objekt tooskip. Hello följande exempel varje sida som visar hello bredvid 15 objekt visas med hello inkrementell hoppar i hello `$skip` parameter.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Layout
På en sida med sökresultat, kanske du vill tooshow en miniatyrbild, en delmängd av fält och en länk tooa fullständiga produktsida.

 ![][2]

I Azure Search kan du använda `$select` och en sökning kommandot tooimplement upplevelsen.

tooreturn en delmängd av fält för en sida vid sida layout:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Bilder och mediafiler är inte direkt sökbara och ska lagras i en annan plattform för lagring, till exempel Azure Blob storage tooreduce kostnader. Definiera ett fält som lagrar hello URL-adressen för hello externt innehåll i hello index och dokument. Du kan sedan använda hello fältet som en bildreferens till en. hello URL toohello bilden måste vara i hello dokumentet.

tooretrieve en produktbeskrivning sidan för en **onClick** händelse, Använd [sökning dokumentet](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass i hello dokumentet tooretrieve hello nyckeln. hello datatyp hello nyckel är `Edm.String`. I det här exemplet är det *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Sortera efter relevans, klassificering eller pris
Sorteringsordningar ofta toorelevance som standard, men det är vanliga toomake alternativa sorteringsordningar är tillgänglig så att kunder kan snabbt blandar befintliga resultat i en annan rank ordning.

 ![][3]

I Azure Search sortering baseras på hello `$orderby` uttryck för alla fält som indexeras som`"Sortable": true.`

Relevans är starkt kopplat till bedömningen profiler. Du kan använda hello standard poäng, som förlitar sig på text analys och statistik toorank ordning alla resultat med högre poäng fortsätter toodocuments med flera eller starkare matchar en sökterm.

Alternativa sorteringsordningar är vanligtvis kopplad till **onClick** händelser som motringning tooa metod som skapar hello sorteringsordning. Till exempel få den här sidelement:

 ![][4]

Du skapar en metod som accepterar hello valt sorteringsalternativet som indata och returnerar en sorterad lista för hello villkor som är associerade med det alternativet.

 ![][5]

> [!NOTE]
> Hello standard bedömningen är tillräcklig för många scenarier, rekommenderar vi basera relevans på en anpassad bedömningsprofilen i stället. En anpassad bedömningsprofilen ger dig ett sätt tooboost objekt som är bättre tooyour företag. Se [lägga till en bedömningsprofilen](http://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information. 
> 
> 

## <a name="faceted-navigation"></a>Aspektbaserad navigering
Sök navigering är vanliga på en resultatsida ofta finns på hello-sida eller en sida. I Azure Search ger fasetterad navigering automatisk dirigerad sökningen baserat på fördefinierade filter. Se [fasetterad navigering i Azure Search](search-faceted-navigation.md) mer information.

## <a name="filters-at-hello-page-level"></a>Filter på hello sidan nivå
Om lösningsdesignen ingår dedikerade söksidor för specifika typer av innehåll (till exempel ett online retail program som har avdelningar som nämns i hello överst på sidan för hello), kan du infoga ett filteruttryck tillsammans med en **onClick** händelse tooopen en sida i en filtrerad tillstånd. 

Du kan skicka ett filter med eller utan ett sökuttryck. Till exempel filtrerar hello följande begäran på varumärke, returnerar endast de dokument som matchar den.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Se [Sök dokument (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) för mer information om `$filter` uttryck.

## <a name="see-also"></a>Se även
* [Azure Söktjänsts-REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [Indexåtgärder](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [Dokumentet åtgärder](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [Video och självstudier om Azure Search](search-video-demo-tutorial-list.md)
* [Fasetterad navigering i Azure Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
