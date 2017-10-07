---
title: aaaIndexers i Azure Search | Microsoft Docs
description: "Crawlar Azure SQL-databasen, Azure Cosmos DB eller Azure storage tooextract sökbara data och fylla i ett Azure Search-index."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 34a7694c-8fd9-46b1-8900-cefdd7236323
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: 6a816252ec5d6032491a12651c05cb1fe77d3d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="indexers-in-azure-search"></a>Indexerare i Azure Search
> [!div class="op_single_selector"]
>
> * [Översikt](search-indexer-overview.md)
> * [Portalen](search-import-data-portal.md)
> * [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
> * [Azure Cosmos DB](search-howto-index-documentdb.md)
> * [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
> * [Azure Table Storage](search-howto-indexing-azure-tables.md)
>
>

En **indexeraren** i Azure Search är en crawler som sökbara data och metadata från en extern datakälla och fyller i ett index baserat på-fält mappningar mellan hello index och datakälla. Den här metoden är ibland hänvisade tooas en pull modell eftersom hello service hämtar data i utan att behöva toowrite all kod som skickar data tooan index.

Du kan använda en indexerare som hello som enda metod för datapåfyllning eller använda en kombination av metoder som omfattar hello användning av en indexerare för att läsa in några av hello fält i ditt index.

Du kan köra indexerare på begäran eller enligt ett återkommande datauppdateringsschema som körs så ofta som var 15:e minut. Mer frekventa uppdateringar kräver en push-modell som uppdaterar data i Azure Search och i din externa datakälla samtidigt.

## <a name="approaches-for-creating-and-managing-indexers"></a>Metoder för att skapa och hantera indexerare
För allmänt tillgängliga indexerare, som Azure SQL och Azure Cosmos DB, kan du skapa och hantera indexerare med hjälp av följande metoder:

* [Portal > Guiden Importera Data](search-get-started-portal.md)
* [Tjänsten REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
* [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

## <a name="basic-configuration-steps"></a>Grundläggande konfigurationssteg
Indexerare kan erbjuda funktioner som är unika toohello datakälla. I detta avseende varierar vissa aspekter av indexerarna och datakällskonfigurationen kan variera efter indexerartyp. Men alla indexerare dela hello samma grundläggande sammansättning och krav. Steg som är vanliga tooall indexerare beskrivs nedan.

### <a name="step-1-create-an-index"></a>Steg 1: Skapa ett index
En indexerare kommer automatisera vissa uppgifter relaterade toodata införandet, men att skapa ett index är inte en av dem. Som krav måste du ha ett fördefinierat index med fält som matchar de i din externa datakälla. Mer information om att strukturera ett index finns i [Skapa ett Index (REST-API för Azure Search)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### <a name="step-2-create-a-data-source"></a>Steg 2: Skapa en datakälla
En indexerare hämtar data från en **datakälla** som innehåller information som till exempel en anslutningssträng. För närvarande stöds hello följande datakällor:

* [Azure SQL Database (eller SQL Server på en virtuell Azure-dator)](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Azure Blob storage](search-howto-indexing-azure-blob-storage.md), används tooextract text från PDF, Office-dokument, HTML eller XML
* [Azure Table Storage](search-howto-indexing-azure-tables.md)

Datakällor konfigureras och hanteras oberoende av hello indexerare som använder dem, vilket innebär en datakälla som kan användas av flera indexerare tooload mer än ett index i taget.

### <a name="step-3create-and-schedule-hello-indexer"></a>Steg 3: skapa och schemalägga hello indexeraren
hello indexeraren definition är en konstruktion som att ange hello index, datakälla och ett schema. En indexerare kan referera en datakälla från en annan tjänst som att datakällan är från hello samma prenumeration. Mer information om att strukturera en indexerare finns i [Skapa et indexerare (REST-API för Azure Search)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## <a name="next-steps"></a>Nästa steg
Nu när du har hello Grundtanken är hello nästa steg tooreview krav och uppgifter specifika tooeach typ av datakälla.

* [Azure SQL Database (eller SQL Server på en virtuell Azure-dator)](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-documentdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md), används tooextract text från PDF, Office-dokument, HTML eller XML
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indexering CSV blobar med hjälp av hello Azure Search Blob indexeraren](search-howto-index-csv-blobs.md)
* [Indexera JSON-blobbar med Azure Search Blob-indexeraren](search-howto-index-json-blobs.md)
