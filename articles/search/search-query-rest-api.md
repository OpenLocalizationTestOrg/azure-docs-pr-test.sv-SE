---
title: "aaa ”fråga ett index (REST API - Azure Search) | Microsoft Docs ”"
description: "Skapa en sökfråga i Azure search och använda Sök parametrar toofilter och sortera sökresultaten."
services: search
documentationcenter: 
manager: jhubbard
author: ashmaka
ms.assetid: 8b3ca890-2f5f-44b6-a140-6cb676fc2c9c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/12/2017
ms.author: ashmaka
ms.openlocfilehash: 2f12238b8f4b045f536489cfc8766fb68307bbe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-rest-api"></a>Fråga ditt Azure Search-index med hello REST API
> [!div class="op_single_selector"]
>
> * [Översikt](search-query-overview.md)
> * [Portalen](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Den här artikeln visar hur tooquery ett index med hjälp av hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).

Innan du påbörjar den här genomgången bör du redan ha [skapat ett Azure Search-index](search-what-is-an-index.md) och [fyllt det med data](search-what-is-data-import.md). Mer bakgrundsinformation finns i [Hur fullständig textsökning fungerar i Azure Search](search-lucene-query-architecture.md).

## <a name="identify-your-azure-search-services-query-api-key"></a>Identifiera API-frågenyckeln för Azure Search-tjänsten
En viktig del av varje search-åtgärd mot hello Azure Search REST API är hello *api-nyckel* som genererades för hello-tjänsten som du har etablerats. Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.

1. toofind din tjänst api-nycklar, du kan logga in toohello [Azure-portalen](https://portal.azure.com/)
2. Gå tooyour Azure Search service-bladet
3. Klicka på ikonen för hello ”nycklar”

Tjänsten har *administratörsnycklar* och *frågenycklar*.

* Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor. Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.
* Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.

Du kan använda en av din fråga nycklar för hello frågor till ett index är. Admin-nycklar kan också användas för frågor, men du bör använda en fråga nyckel i din programkod eftersom det bättre följer hello [principen om minsta behörighet](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="formulate-your-query"></a>Formulera frågan
Det finns två sätt för[söka ditt index med hello REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Ett sätt är tooissue en HTTP POST-begäran där din frågeparametrar definieras i ett JSON-objekt i hello begärandetexten. hello annat sätt är tooissue en HTTP GET-begäran där din frågeparametrar definieras inom hello URL-begäran. POST har flera [mjukas upp gränser](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) på Frågeparametrar än GET hello storlek. Av den anledningen rekommenderar vi att du använder POST såvida det inte finns särskilda omständigheter som gör att GET är lämpligare.

För både POST och GET måste tooprovide din *tjänstnamnet*, *Indexnamnet*, och hello rätt *API-versionen* (hello aktuella API-versionen är `2016-09-01` hello tidpunkt publicera det här dokumentet) i hello URL-begäran. GET, hello *frågesträng* på hello slutet av hello URL kan du ge frågeparametrar Hej. Nedan följer hello URL-format:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

hello-format för POST är hello samma, men med endast api-version i hello frågan string-parametrar.

#### <a name="example-queries"></a>Exempelfrågor
Här är några exempelfrågor för ett index med namnet ”hotels”. Frågorna visas i både GET- och POST-format.

Sökas hello termen ”budget' hello hela indexet och returnerar bara hello `hotelName` fält:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

Tillämpa ett filter toohello index toofind hotell billigare än 150 USD per natt och returnera hello `hotelId` och `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Sök hello hela index, efter ett visst fält (`lastRenovationDate`) i fallande ordning, ta hello två översta resultat och visa endast `hotelName` och `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="submit-your-http-request"></a>Skicka HTTP-begäran
Nu när du har formulerat frågan som en del av HTTP-begärans URL (för GET) eller brödtext (för POST) kan du definiera begärandehuvudena och skicka din fråga.

#### <a name="request-and-request-headers"></a>Begäran och begärandehuvuden
Du måste definiera två begärandehuvuden för GET, eller tre för POST:

1. Hej `api-key` huvud måste anges toohello Frågenyckeln du hittade i steg I ovan. Du kan också använda en administrationsnyckel som hello `api-key` huvud, men det rekommenderas att du använder en nyckel för frågan som uteslutande beviljas läsbehörighet tooindexes och dokument.
2. Hej `Accept` huvud måste anges för`application/json`.
3. POST, hello `Content-Type` huvud också ställas in för`application/json`.

Nedan finns en HTTP GET-begäran toosearch hello ”hotell” index med hello Azure Search REST-API, med hjälp av en enkel fråga som söker efter hello termen ”översikt”:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

Här är hello samma exempelfråga nu med hjälp av HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

En lyckad fråga resulterar i en statuskod för `200 OK` och hello sökresultaten visas som JSON i hello svarstexten. Här är vad hello resultat för hello ovan frågan ser ut som, förutsatt att hello ”hotell” index fylls med hello exempeldata i [Import av Data i Azure Search med hello REST API](search-import-data-rest-api.md) (Observera att hello JSON har formaterats för tydlighetens skull).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

toolearn fler besök hello ”” avsnittet av [Sök dokument](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Mer information om andra HTTP-statuskoder som kan returneras om det uppstår fel finns i [HTTP-statuskoder (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).
