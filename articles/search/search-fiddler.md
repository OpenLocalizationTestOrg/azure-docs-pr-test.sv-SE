---
title: 'aaaHow toouse Fiddler tooevaluate och testa Azure Search REST API: er | Microsoft Docs'
description: "Använd Fiddler för en kod utan metoden tooverifying Azure Search tillgänglighet och testar hello REST API: er."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
ms.openlocfilehash: 2912e1180717d7b40a1e4f7f7f00daf2cc254f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-fiddler-tooevaluate-and-test-azure-search-rest-apis"></a>Använd Fiddler tooevaluate och testa Azure Search REST API: er
> [!div class="op_single_selector"]
>
> * [Översikt](search-query-overview.md)
> * [Sökutforskaren](search-explorer.md)
> * [Fiddler](search-fiddler.md)
> * [NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Den här artikeln förklarar hur toouse Fiddler, tillgänglig som en [av Telerik](http://www.telerik.com/fiddler), tooissue HTTP-begäranden tooand Visa svar med hello Azure Search REST-API, utan att behöva toowrite någon kod. Azure Search är en fullständigt hanterad, värd- och molnbaserad söktjänst i Microsoft Azure, som enkelt kan programmeras via .NET och REST-API:er. hello Azure Search-tjänsten REST API: er som finns dokumenterade i [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

I följande hello, ska du skapa ett index, ladda upp dokument, fråga hello index och sedan frågan hello system information om.

toocomplete dessa steg, behöver du en Azure Search-tjänst och `api-key`. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) anvisningar för hur tooget startas.

## <a name="create-an-index"></a>Skapa ett index
1. Starta Fiddler. På hello **filen** menyn inaktivera **fånga in trafik** toohide överflödig http-aktivitet som är inte relaterat toohello aktuella aktiviteten.
2. På hello **Composer** fliken ska du formulerar en begäran som ser ut som följande skärmbild som visar hello.

      ![][1]
3. Välj **PUT**.
4. Ange en URL som anger hello-tjänstens URL, attribut och hello api-versionen. Några pekare tookeep i åtanke:

   * Använd HTTPS som hello prefix.
   * Attributet för begäran är ”/indexes/hotels”. Detta talar om sökningen toocreate ett index med namnet 'hotell'.
   * API-versionen anges i gemener, i följande format: ”?api-version=2016-09-01”. API-versioner är viktiga eftersom Azure Search distribuerar uppdateringar regelbundet. I sällsynta fall kan en tjänstuppdatering införa en ny ändring toohello API. Av den anledningen kräver Azure Search en API-version i varje begäran så att du har full kontroll över vilken version som används.

     hello fullständig URL bör se ut ungefär toohello följande exempel.

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. Ange hello begärandehuvudet ersätter hello värden och api-nyckel med värden som är giltiga för din tjänst.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. Brödtext i begäran, klistra in i hello fält som utgör hello indexdefinitionen.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }
7. Klicka på **Kör**.

Om några sekunder bör du se ett HTTP-201 svar i hello session listan, som anger hello indexet har skapats.

Om du får HTTP 504 Kontrollera hello URL anger HTTPS. Om du ser HTTP 400 eller 404 Kontrollera hello begäran brödtext tooverify det har inga kopiera / klistra in-fel. Ett HTTP 403 tyder vanligtvis på problem med hello api-nyckel (en ogiltig nyckel eller ett syntax problem med hur hello api-nyckel har angetts).

## <a name="load-documents"></a>Läsa in dokument
På hello **Composer** fliken begäran toopost dokument kommer att se ut hello följande. hello innehåller hello begäran hello sökdata för 4 hotell.

   ![][2]

1. Välj **POST**.
2. Ange en URL som börjar med HTTPS, följt av tjänstens URL, följt av ”/indexes/<'indexname'>/docs/index?api-version=2016-09-01”. hello fullständig URL bör se ut ungefär toohello följande exempel.

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. Huvudet i begäran bör vara hello samma som tidigare. Kom ihåg att du har ersatt hello värden och api-nyckel med värden som är giltiga för din tjänst.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. hello brödtext i begäran innehåller fyra dokument toobe tillagda toohello hotell index.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
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
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be hello one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }
5. Klicka på **Kör**.

Du bör se ett 200 HTTP-svar i hello session listan efter några sekunder. Detta anger hello dokument har skapats. Om du får en 207 misslyckades tooupload minst ett dokument. Om du får ett 404 har ett syntaxfel i hello sidhuvud eller hello begäran om.

## <a name="query-hello-index"></a>Frågan hello index
Nu när ett index och dokument har lästs in kan du skicka frågor mot dem.  På hello **Composer** fliken en **hämta** liknande toohello följande skärmdump ser kommandot som frågar din tjänst.

   ![][3]

1. Välj **GET**.
2. Ange en URL som börjar med HTTPS, följt av tjänstens URL, följt av ”/indexes/<'indexname'>/docs?”, följt av frågeparametrar. Använd hello följande URL, ersätter hello exempel värdnamn med något som gäller för din tjänst som exempel.

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   Den här frågan söker efter hello termen ”översikt” och hämtar aspekten kategorier för klassificering.
3. Huvudet i begäran bör vara hello samma som tidigare. Kom ihåg att du har ersatt hello värden och api-nyckel med värden som är giltiga för din tjänst.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

hello svarskoden bör vara 200 och hello svarsutdata bör se ut ungefär toohello följande skärmdump.

   ![][4]

hello följande exempelfråga är från hello [sökindex åtgärden (Azure Search-API)](http://msdn.microsoft.com/library/dn798927.aspx) på MSDN. Många av hello exempel frågorna i det här avsnittet innehåller blanksteg, vilket inte är tillåtna i Fiddler. Ersätt varje utrymme med en + tecknet innan klistra in hello frågesträng innan du försöker hello fråga i Fiddler.

**Innan blankstegen har ersatts:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

**När blankstegen har ersatts med +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-hello-system"></a>frågan hello system
Du kan också fråga hello system tooget antal och lagring dokumentanvändning. På hello **Composer** fliken din begäran kommer att se liknande toohello följande och hello svaret returnerar antalet hello antalet dokument och diskutrymme som används.

 ![][5]

1. Välj **GET**.
2. Ange en URL som innehåller tjänstens URL, följt av ”/indexes/hotels/stats?api-version=2016-09-01”:

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. Ange hello begärandehuvudet ersätter hello värden och api-nyckel med värden som är giltiga för din tjänst.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Lämna hello begärandetexten tomt.
5. Klicka på **Kör**. Du bör se en 200 HTTP-statuskod i hello sessionslista. Välj hello post för ditt kommando.
6. Klicka på hello **kontrollanter** klickar du på hello **huvuden** fliken och markera sedan hello JSON-format. Du bör se hello antal och lagring storlek (i KB).

## <a name="next-steps"></a>Nästa steg
Se [hantera din söktjänst på Azure](search-manage.md) för en ingen kod metoden toomanaging och använda Azure Search.

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
