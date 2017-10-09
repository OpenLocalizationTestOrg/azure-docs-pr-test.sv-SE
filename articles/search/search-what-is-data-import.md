---
title: "aaaData överför i Azure Search | Microsoft Docs"
description: "Lär dig hur tooupload data tooan index i Azure Search."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: ashmaka
ms.openlocfilehash: a95eae94f72c1d0926804ff7e1152f21773fcabf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search"></a>Ladda upp data tooAzure sökning
> [!div class="op_single_selector"]
> * [Översikt](search-what-is-data-import.md)
> * [NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Det finns två sätt toopopulate ett index med dina data. hello första alternativet manuellt gör dina data till hello index med hello Azure Search [REST API](search-import-data-rest-api.md) eller [.NET SDK](search-import-data-dotnet.md). hello andra alternativet är för[peka en datakälla som stöds](search-indexer-overview.md) tooyour index och låta Azure Search automatiskt hämtar hello data.

## <a name="push-data-tooan-index"></a>Push data tooan index
Den här metoden refererar tooprogrammatically skicka dina data tooAzure Sök toomake den tillgänglig för sökning. För program med mycket låg svarstidskrav (t.ex, om du behöver söker operations toobe synkroniserad med dynamiska inventering databaser), är hello push modellen ditt enda alternativ.

Du kan använda hello [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) eller [.NET SDK](search-import-data-dotnet.md) toopush data tooan index. Det finns inget verktyg stöd för att skicka data via hello-portalen.

Den här metoden är mer flexibelt än hello pull modellen eftersom du kan ladda upp dokument individuellt eller i batchar (in too1000 per batch eller 16 MB, oavsett vilken gränsen kommer först). hello push-modell kan du också tooupload dokument tooAzure Sök oavsett var informationen finns.

alla dokument i hello dataset måste ha fält för att mappa toofields som definierats i din indexeringsschema hello dataformat tolkas av Azure Search är JSON. 

## <a name="pull-data-into-an-index"></a>Hämta in data till ett index
hello pull modellen crawlar en datakälla som stöds och överför automatiskt hello data till ditt index. I Azure Search implementeras den här funktionen genom *indexerare*, som för närvarande är tillgängliga för [Blob Storage](search-howto-indexing-azure-blob-storage.md), [Table Storage](search-howto-indexing-azure-tables.md), [Azure Cosmos DB](http://aka.ms/documentdb-search-indexer), [Azure SQL Database och SQL Server på virtuella datorer i Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 

Indexerare ansluta ett index tooa datakällan (vanligtvis en tabeller, vyer och motsvarande struktur) och mappa källfälten fält tooequivalent i hello index. Vid körning, hello raduppsättningen är automatiskt omvandlade tooJSON och läses in i hello angivet index. Alla indexerare stöd för schemaläggning så att du kan ange hur ofta hello data toobe uppdateras. De flesta indexerare ange ändringsspårning om hello datakällan stöder den. Genom att spåra ändringar och borttagningar tooexisting dokument dessutom toorecognizing nya dokument, indexerare tar bort behovet av hello tooactively hantera hello data i ditt index. 

Indexerare funktioner som exponeras i hello [Azure-portalen](search-import-data-portal.md), hello [REST API](/rest/api/searchservice/Indexer-operations), och hello [.NET SDK](/dotnet/api/microsoft.azure.search.indexersoperations). 

En fördel toousing hello portal är att Azure Search kan vanligtvis generera ett standardschema index för du genom att läsa hello metadata för hello källa dataset. Du kan ändra hello genererade indexet tills hello index bearbetas, efter vilken hello tillåts endast schema redigeringar är de som inte kräver indexeringen. Om hello ändringarna toomake inverkan hello schema direkt, behöver du toorebuild hello index. 

När hello index är ifylld, kan du använda **Sök Explorer** i hello portal kommandofältet som en verifiering av steg.

## <a name="query-an-index-using-search-explorer"></a>Köra frågor mot ett index med hjälp av Sökutforskaren

Ett snabbt sätt tooperform en preliminär kontroll på hello dokumentöverföring är toouse **Sök Explorer** hello-portalen. hello explorer kan du fråga ett index utan att behöva toowrite någon kod. hello sökinställningar är baserat på standardinställningarna, till exempel hello [enkel syntax](/rest/api/searchservice/simple-query-syntax-in-azure-search) och standard [searchMode frågeparameter](/rest/api/searchservice/search-documents). Resultaten returneras i JSON så att du kan inspektera hello hela dokumentet.

> [!TIP]
> Ett stort antal [kodexempel för Azure Search](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) innehåller inbäddade eller tillgängliga datauppsättningar, erbjuder ett enkelt sätt tooget igång. hello-portalen innehåller också ett exempel indexerare och datakällan som består av en liten fastigheter dataset (med namnet ”realestate-oss-sample”). När du kör hello förkonfigurerade indexeraren hello datakälla för exempel på ett index skapas och laddas med dokument som kan efterfrågas i Sök Explorer eller av kod som du skriver.
