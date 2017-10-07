---
title: "aaa ”ladda upp data (REST API - Azure Search) | Microsoft Docs ”"
description: "Lär dig hur tooupload data tooan index i Azure Search med hello REST API."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: 8d0749fb-6e08-4a17-8cd3-1a215138abc6
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 6ba1336012d1f0f6d6d6c933e16aa879afb9b824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-rest-api"></a>Överför data tooAzure Sök med hjälp av hello REST API
> [!div class="op_single_selector"]
>
> * [Översikt](search-what-is-data-import.md)
> * [NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
>
>

Den här artikeln visar hur toouse hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) tooimport data till ett Azure Search-index.

Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md).

I toopush dokument till ditt index med hello REST-API, utfärdar en HTTP POST-begäran tooyour indexet URL-slutpunkt. hello brödtext hello HTTP-begäran meddelandetexten är en JSON-objekt som innehåller hello dokument toobe lagts till, ändras eller tas bort.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Identifiera Azure Search-tjänstens API-administratörsnyckel
Vid utfärdande av HTTP-begäranden mot din tjänst med hjälp av hello REST API *varje* API-begäran måste innehålla hello api-nyckel som har genererats för hello söktjänsten du etablerat. Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.

1. toofind din tjänst api-nycklar, du kan logga in toohello [Azure-portalen](https://portal.azure.com/)
2. Gå tooyour Azure Search service-bladet
3. Klicka på hello ”nycklar”-ikon

Tjänsten har *administratörsnycklar* och *frågenycklar*.

* Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor. Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.
* Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.

För hello importerar data till ett index är kan du använda antingen primär eller sekundär administrationsnyckel.

## <a name="decide-which-indexing-action-toouse"></a>Bestäm vilka indexering åtgärden toouse
När du använder hello REST API utfärdar HTTP POST-begäranden med JSON-begäran organ tooyour Azure Search indexet slutpunkts-URL. hello JSON-objekt i HTTP-begärandetexten innehåller en JSON-matris med namnet ”värde” som innehåller JSON-objekt som representerar dokument som du vill att tooadd tooyour index, uppdatera eller ta bort.

Varje JSON-objekt i hello ”värde” matrisen representerar en toobe för dokument som indexeras. De här objekten innehåller hello Dokumentnyckel och anger hello önskad indexering åtgärd (överföringen, merge, ta bort och så vidare). Beroende på vilka hello under åtgärder som du väljer endast vissa fält måste inkluderas för varje dokument:

| @search.action | Beskrivning | Nödvändiga fält för varje dokument | Anteckningar |
| --- | --- | --- | --- |
| `upload` |En `upload` åtgärden är liknande tooan ”upsert” där hello dokumentet infogas om det är nytt och uppdatera/ersättas om den finns. |nyckel plus andra fält som du vill toodefine |När du uppdaterar och ersätta ett befintligt dokument, alla fält som anges i begäran hello har dess fält som har angetts för`null`. Detta inträffar även när hello fältet tidigare var inställt på tooa icke-null-värde. |
| `merge` |Uppdateringar som har ett befintligt dokument med hello angivna fält. Om hello dokumentet inte finns i hello index, misslyckas hello sammanslagning. |nyckel plus andra fält som du vill toodefine |Alla fält som du anger i en merge ersätter hello befintliga fält i hello. Detta gäller även fält av typen `Collection(Edm.String)`. Till exempel om hello dokumentet innehåller ett fält `tags` med värdet `["budget"]` och du utföra en koppling med värdet `["economy", "pool"]` för `tags`, hello slutliga värdet för hello `tags` fältet kommer att vara `["economy", "pool"]`. Det blir inte `["budget", "economy", "pool"]`. |
| `mergeOrUpload` |Den här åtgärden fungerar som `merge` om ett dokument med hello anges nyckeln redan finns i hello index. Om hello dokumentet inte finns, fungerar den som `upload` med ett nytt dokument. |nyckel plus andra fält som du vill toodefine |- |
| `delete` |Tar bort hello angivna dokumentet från hello index. |endast nyckel |Alla fält som du anger än hello nyckelfältet kommer att ignoreras. Om du vill tooremove ett fält från ett dokument, använda `merge` enkelt och i stället ange hello fältet explicit toonull. |

## <a name="construct-your-http-request-and-request-body"></a>Skapa HTTP-begäran och begärandetexten
Nu när du har samlat in hello nödvändiga fältvärden för Indexåtgärder, är du redo tooconstruct hello faktiska HTTP-begäran och JSON begära brödtext tooimport dina data.

#### <a name="request-and-request-headers"></a>Begäran och begärandehuvuden
I hello URL, behöver du tooprovide ditt namn, indexnamn (”hotell” i det här fallet), samt hello rätt API-versionen (hello aktuella API-versionen är `2016-09-01` när hello publicera det här dokumentet). Du behöver toodefine hello `Content-Type` och `api-key` begärandehuvuden. Använd en av din tjänst admin nycklar för hello senare.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Begärandetext
```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close tootown hall and hello river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

I detta fall använder vi `upload`, `mergeOrUpload` och `delete` som våra sökåtgärder.

Anta att exempelindexet ”hotels” redan fyllts med ett antal dokument. Observera hur vi hade inte toospecify alla fält för hello möjliga dokument när du använder `mergeOrUpload` och hur vi bara ange hello Dokumentnyckel (`hotelId`) när du använder `delete`.

Dessutom Observera att du får bara innehålla too1000 dokument (eller 16 MB) i en enskild indexering begäran.

## <a name="understand-your-http-response-code"></a>Förstå HTTP-svarskoden
#### <a name="200"></a>200
När du har skickat en lyckad indexeringsbegäran får du ett HTTP-svar med statuskoden `200 OK`. hello JSON-meddelandetext av hello HTTP-svar är följande:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Statuskoden `207` returneras om minst ett objekt inte indexerades. hello JSON-meddelandetext av hello HTTP-svar som innehåller information om misslyckade hello-dokument.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "hello search service is too busy tooprocess this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> Detta innebär ofta att hello belastningen på din sökning tjänsten når en plats där indexering begäranden börjar tooreturn `503` svar. I detta fall rekommenderar vi starkt att klientkoden stannar upp och väntar innan ett nytt försök görs. Detta ger hello system vissa tid toorecover, öka hello risken att framtida begäranden ska lyckas. Du försöker dina begäranden snabbt kommer bara förlänga hello situation.
>
>

#### <a name="429"></a>429
Statuskoden `429` returneras när du har överskridit kvoten för hello antalet dokument per index.

#### <a name="503"></a>503
Statuskoden `503` returneras om ingen av hello objekt i hello-begäran har indexerades. Felet innebär att hello systemet är hårt belastad och din begäran kan inte bearbetas just nu.

> [!NOTE]
> I detta fall rekommenderar vi starkt att klientkoden stannar upp och väntar innan ett nytt försök görs. Detta ger hello system vissa tid toorecover, öka hello risken att framtida begäranden ska lyckas. Du försöker dina begäranden snabbt kommer bara förlänga hello situation.
>
>

Mer information om dokumentåtgärder och svar om lyckade/misslyckade åtgärder finns i [Lägga till, uppdatera eller ta bort dokument](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents). Mer information om andra HTTP-statuskoder som kan returneras om det uppstår fel finns i [HTTP-statuskoder (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

## <a name="next-steps"></a>Nästa steg
Du kommer att redo toostart utfärda frågor toosearch för dokument efter fylla ditt Azure Search-index. Mer information finns i [Fråga ditt Azure Search-index](search-query-overview.md).
