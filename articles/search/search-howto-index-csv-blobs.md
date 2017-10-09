---
title: "aaaIndexing CSV-blobbar med Azure blob sökindexeraren | Microsoft Docs"
description: "Lär dig hur tooindex CSV-blobbar med Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: ed3c9cff-1946-4af2-a05a-5e0b3d61eb25
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/15/2016
ms.author: eugenesh
ms.openlocfilehash: f2b1ce824e62c5b3f6155c6e88887897cf1a8eae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexering CSV blobbar med Azure Search blob indexeraren
Som standard [Azure blob sökindexeraren](search-howto-indexing-azure-blob-storage.md) Parsar avgränsade text blobbar som ett enda segment av text. Men med blobbar som innehåller CSV-data, vill du ofta tootreat varje rad i hello blob som ett separat dokument. Den angivna hello avgränsad exempelvis följande text: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Du kanske vill tooparse den i 2 dokument som var och en innehåller fälten ”taggar”, ”datePublished” och ”id”.

I den här artikeln du lära dig hur tooparse CSV BLOB-objekt med en indexerare för Azure Search-blob. 

> [!IMPORTANT]
> Den här funktionen är för närvarande under förhandsgranskning. Det är endast tillgänglig i hello REST-API med version **2015-02-28-Preview**. Kontrollera komma ihåg, förhandsgranska API: er är avsedd för testning och utvärdering och ska inte användas i produktionsmiljöer. 
> 
> 

## <a name="setting-up-csv-indexing"></a>Konfigurera CSV-indexering
tooindex CSV blobbar, skapa eller uppdatera en indexerare definition med hello `delimitedText` parsning läge:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Mer information om Hej skapa indexeraren API, kolla [skapa indexeraren](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Anger att hello (icke-tomt) främsta varje blobb innehåller huvuden.
Om BLOB får inte innehålla en inledande rubrikraden, anges hello huvuden i hello indexeraren konfiguration: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

För närvarande stöds endast hello UTF-8-kodning. Hej också endast komma `','` tecken stöds eftersom hello avgränsare. Om du behöver stöd för andra kodningar eller avgränsare berätta på [våra UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> När du använder hello avgränsade text parsning Azure Search-läge förutsätter att alla blobbar i datakällan kommer att CSV. Om du behöver toosupport en blandning av CSV- och icke-CSV-blobbar i hello samma datakälla, låt oss på [våra UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Exempel på begäran
Om detta alla tillsammans, här är hello fullständig nyttolast exempel. 

Datakällan: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexerare:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Hjälp oss att förbättra Azure Search
Om du har funktionsförfrågningar om eller förslag på förbättringar kan du nå toous på vår [UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search/).

