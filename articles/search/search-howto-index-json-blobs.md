---
title: aaaIndexing JSON-blobbar med Azure Search blob indexeraren
description: Indexering JSON-blobbar med Azure Search blob indexeraren
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: 269968714358cd40ea66863b4dbb97766e1d77e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexering JSON-blobbar med Azure Search blob indexeraren
Den här artikeln visar hur tooconfigure Azure Search blob indexeraren tooextract strukturerad innehåll från BLOB som innehåller JSON.

## <a name="scenarios"></a>Scenarier
Som standard [Azure blob sökindexeraren](search-howto-indexing-azure-blob-storage.md) Parsar JSON-blobbar som ett enda segment av text. Ofta vill du toopreserve hello strukturen för JSON-dokument. Till exempel få hello JSON-dokumentet

    {
        "article" : {
             "text" : "A hopefully useful article explaining how tooparse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Du kanske vill tooparse den till en Azure Search-dokument med ”text”, ”datePublished” och ”-taggar” fält.

När dina blobbar också innehålla en **matris av JSON-objekt**, kanske du vill att varje element i hello matris toobecome ett separat Azure Search-dokument. Till exempel få en blob med JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Du kan fylla i ditt Azure Search index med tre separata dokument med fälten ”id” och ”text”.

> [!IMPORTANT]
> hello JSON-matris parsning funktioner är för närvarande under förhandsgranskning. Det är endast tillgänglig i hello REST-API med version **2015-02-28-Preview**. Kom ihåg, förhandsgranska API: er är avsedd för testning och utvärdering och ska inte användas i produktionsmiljöer.
>
>

## <a name="setting-up-json-indexing"></a>Ställa in JSON indexering
Indexering JSON-blobbar är liknande toohello vanligt dokument extrahering. Skapa först hello datasource exakt på vanligt sätt: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Skapa sedan hello mål sökindex om du inte redan har ett. 

Slutligen skapar du en indexerare och anger hello `parsingMode` parameter för`json` (tooindex varje blob som ett enskilt dokument) eller `jsonArray` (om dina blobbar innehåller JSON-matriser och du måste varje element i en matris toobe behandlas som ett separat dokument):

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Om det behövs, Använd **fältet mappningar** toopick hello egenskaper för hello källa JSON-dokument som används för toopopulate dina mål sökindex enligt hello nästa avsnitt.

> [!IMPORTANT]
> När du använder `json` eller `jsonArray` parsning läge Azure Search förutsätter att alla blobbar i datakällan innehåller JSON. Om du behöver toosupport en blandning av JSON och icke-JSON-blobbar i hello samma datakälla, berätta för oss på [våra UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-toobuild-search-documents"></a>Med hjälp av fältet mappningar toobuild Sök dokument
För närvarande kan Azure Search indexera godtyckliga JSON-dokument direkt, eftersom den stöder endast primitiva datatyper, string-matriser och GeoJSON punkter. Du kan dock använda **fältet mappningar** toopick delar av din JSON-dokumentet och ”lyfta” dem i översta hello Sök efter dokument-fält. toolearn om fältet mappningar grundläggande finns [Azure Search indexeraren fältmappningar överbrygga hello skillnaderna mellan datakällor och sökindexen](search-indexer-field-mappings.md).

Komma tillbaka tooour exempel JSON-dokumentet:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how tooparse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Anta att du har en sökindex med hello följande fält: `text` av typen `Edm.String`, `date` av typen `Edm.DateTimeOffset`, och `tags` av typen `Collection(Edm.String)`. toomap din JSON till hello önskad form använder hello följande fältmappningar:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

hello källa fältnamnen i hello mappningar anges med hello [JSON pekaren](http://tools.ietf.org/html/rfc6901) notation. Du startar med ett snedstreck toorefer toohello roten för JSON-dokumentet och välj önskad hello egenskap (på valfri nivå av kapsling) genom att använda vanlig snedstreck kommaavgränsade sökväg.

Med hjälp av en nollbaserade index kan du också läsa tooindividual matriselementen. Till exempel toopick hello första elementet i hello ”taggar” array från hello ovan exempelvis använda en avbildning så här:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Om fältet källnamnet i en sökväg för mappning av fältet refererar tooa-egenskap som inte finns i JSON, ignoreras mappningen utan fel. Det gör du så att vi kan stödja dokument med ett annat schema (vilket är ett vanligt användningsfall). Eftersom det finns ingen verifiering, måste tootake försiktighet tooavoid skrivfel i din specifikation för mappning av fält.
>
>

Om JSON-dokument innehåller endast enkla översta egenskaper, kanske du inte behöver fältmappningar alls. Till exempel om din JSON ser ut så här hello översta egenskaper ”text”, mappningar ”datePublished” och ”etiketter” direkt toohello motsvarande fält i hello sökindex:

    {
       "text" : "A hopefully useful article explaining how tooparse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Här är en fullständig indexeraren nyttolast med fältmappningar:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="indexing-nested-json-arrays"></a>Indexering kapslade JSON-matriser
Vad händer om du vill tooindex en matris av JSON-objekt, men att matris är kapslat någonstans i hello dokument? Du kan välja vilken egenskap innehåller hello-matris med hello `documentRoot` konfigurationsegenskapen. Om exempelvis dina blobbar se ut så här:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use hello documentRoot property" },
                { "id" : "2", "text" : "toopluck hello array you want tooindex" },
                { "id" : "3", "text" : "even if it's nested inside hello document" }  
            ]
        }
    }

Använd den här konfigurationen tooindex hello-matris i hello `level2` egenskapen:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="help-us-make-azure-search-better"></a>Hjälp oss att förbättra Azure Search
Om du har funktionsförfrågningar om eller förslag på förbättringar, nå ut toous på vår [UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search/).
