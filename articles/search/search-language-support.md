---
title: "aaaAzure Sök flera språk | Microsoft Docs"
description: "Azure Search stöder 56 språk utnyttja språkanalys från Lucene och bearbetning av naturligt språk teknik från Microsoft."
services: search
documentationcenter: 
author: yahnoosh
manager: pablocas
editor: 
ms.assetid: 55a00b44-804d-41bb-9c96-e6ea498616f5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: jlembicz
ms.openlocfilehash: 9a2e567a82ee563521c12ea320f6c484a8e73f04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Skapa ett index för dokument på flera språk i Azure Search
> [!div class="op_single_selector"]
>
> * [Portal](search-language-support.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

Unleashing hello kraften hos språkanalys är så enkel som en egenskap på ett sökbara fält i hello indexdefinitionen. Nu kan du göra det här steget i hello-portalen.

Nedan visas skärmdumpar av hello Azure Portal, blad för Azure Search som tillåter att användare toodefine en indexeringsschema. Användare kan skapa alla hello fält och ange hello analyzer-egenskapen för dem från det här bladet.

> [!IMPORTANT]
> Du kan bara ange en språk analyzer under fältdefinition, som i när du skapar ett nytt index från hello bakgrund eller när du lägger till ett nytt fält tooan befintliga index. Kontrollera att du anger fullständigt alla attribut, inklusive hello analyzer när du skapar hello-fältet. Du kommer inte att kunna tooedit hello attribut eller ändra hello analyzer typ när du sparar ändringarna.
>
>

## <a name="define-a-new-field-definition"></a>Definiera en ny fältdefinition
1. Logga in toohello [Azure-portalen](https://portal.azure.com) och öppna hello service bladet för din söktjänst.
2. Klicka på **Lägg till index** menyraden överst hello i hello service instrumentpanelen toostart ett nytt index eller öppna ett befintligt index tooset en analyzer på nya fält som du vill lägga till i hello kommandot tooan befintligt index.
3. hello fält bladet visas med alternativ för att definiera hello schemat för hello index, inklusive hello Analyzer fliken används för att välja ett språk analyzer.
4. Starta en fältdefinition av i fälten genom att ange ett namn, väljer hello datatyp och anger attributen toomark hello fält som fulltext sökbara, strängfält i sökresultat kan användas i aspekten navigeringsstrukturer kan sorteras och så vidare.
5. Innan du fortsätter toohello nästa fält, öppna hello **Analyzer** fliken.

![][1]
*tooselect en analyzer fliken hello Analyzer på bladet för hello-fält*

## <a name="choose-an-analyzer"></a>Välj en analyzer
1. Rulla toofind hello fält som du definierar.
2. Om du inte har markerats hello fältet sökbart, klickar du på hello kryssrutan nu toomark som **sökbara**.
3. Klicka på hello Analyzer området toodisplay hello lista över tillgängliga analyzers.
4. Välj hello analyzer du vill toouse.

![][2]
*Välj något av hello stöds analyzers för varje fält*

Som standard använder alla sökbara fält hello [Standard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) som är oberoende av språk. tooview hello fullständig lista över stöds analyzers, se [språkstöd i Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).

När hello språk analyzer har valts för ett fält, ska det användas med varje fulltextindexering och begäran för det här fältet. När en fråga skickas ut mot flera fält med olika analyzers kommer hello frågan att behandlas oberoende av hello rätt analyzers för varje fält.

Många webb- och mobilprogram kan du hantera användare runt hello världen med olika språk. Det är möjligt toodefine ett index för ett scenario som detta genom att skapa ett fält för varje språk som stöds.

![][3]
*Indexdefinitionen med en beskrivningsfält för varje språk som stöds*

Om hello språket för hello agenten skickar en fråga kallas en sökbegäran kan vara begränsade tooa specifika fält med hello **searchFields** Frågeparametern. hello utfärdas följande fråga endast mot hello beskrivning i polska:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

Du kan fråga ditt index från hello-portalen med hjälp av **Sök explorer** toopaste i en fråga liknande toohello visas ovan. Sök explorer är tillgänglig från hello kommandofältet hello service-bladet. Se [fråga ditt Azure Search-index i hello portal](search-explorer.md) mer information.

Ibland är hello språket för hello agenten skickar en fråga Okänd, i vilket fall hello fråga kan utfärdas mot alla fält samtidigt. Om det behövs, inställningar för resulterar i ett visst språk kan definieras med [bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx). Matchningar i hello beskrivning på engelska bedömas i hello exemplet nedan högre relativa toomatches på polska och franska:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

Om du är en .NET-utvecklare, Observera att du kan konfigurera språkanalys med hello [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search). senaste hello-versionen har stöd för hello Microsoft språkanalys samt.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
