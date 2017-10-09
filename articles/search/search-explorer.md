---
title: "aaa ”fråga ett index (portal - Azure Search) | Microsoft Docs ”"
description: "Utfärda en sökfråga i hello Azure Portal Sök Explorer."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 07/10/2017
ms.author: ashmaka
ms.openlocfilehash: 56bab3ef8a66eeb053fbbeb6d322acb6824fb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-an-azure-search-index-using-search-explorer-in-hello-azure-portal"></a>Fråga en Azure Search-index med Sök Explorer i hello Azure-portalen
> [!div class="op_single_selector"]
> * [Översikt](search-query-overview.md)
> * [Portalen](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Den här artikeln visar hur tooquery ett Azure Search index med **Sök Explorer** i hello Azure-portalen. Du kan använda Sök Explorer toosubmit enkla eller fullständig Lucene frågan strängar tooany befintligt index i din tjänst.

## <a name="open-hello-service-dashboard"></a>Öppna hello service instrumentpanelen
1. Klicka på **alla resurser** i hello hopp stapel på vänster sida av hello hello [Azure-portalen](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Välj din Azure Search-tjänst.

## <a name="select-an-index"></a>Välj ett index

Välj hello index som toosearch från hello **index** panelen.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Öppna Sökutforskaren

Klicka på hello Sök Explorer panelen tooslide öppna hello sökfältet och resultatfönstret.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Starta sökningen

När du använder hello Sök Explorer kan du ange [fråga parametrar](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) tooformulate hello frågan.

1. Skriv en fråga i **Frågesträng** och tryck sedan på **Sök**. 

   hello frågesträngen är automatiskt parsas till hello rätt begäran URL toosubmit en HTTP-begäran mot hello Azure Search REST API.   
   
   Du kan använda någon giltig enkla eller fullständig Lucene syntax toocreate hello fråga. Hej `*` tecken är likvärdiga tooan tom eller ospecificerad sökning som returnerar alla dokument i bokstavsordning.

2. I **resultat**, frågans resultat visas i raw JSON identiska toohello nyttolast som returneras i en HTTP-svarstexten vid utfärdande av begäranden via programmering.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Nästa steg

hello följande resurser ger information om ytterligare fråga syntax och exempel.

 + [Enkel frågesyntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene-frågesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Exempel på Lucene-frågesyntax](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [Syntax för OData-filteruttryck](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 