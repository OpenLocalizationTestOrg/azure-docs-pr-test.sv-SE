---
title: "aaa ”skapa ett index (REST API - Azure Search) | Microsoft Docs ”"
description: Skapa ett index i kod med hello Azure Search http-REST API.
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: ac6c5fba-ad59-492d-b715-d25a7a7ae051
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 117ab64a9874a443351a8a02a9b959b8f7beb7c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-rest-api"></a>Skapa ett Azure Search-index med hello REST API
> [!div class="op_single_selector"]
>
> * [Översikt](search-what-is-an-index.md)
> * [Portalen](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
>
>

Den här artikeln vägleder dig genom hello processen att skapa ett Azure Search [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) med hello Azure Search REST API.

Innan du följer den här guiden och skapar ett index bör du redan ha [skapat en Azure Search-tjänst](search-create-service-portal.md).

toocreate ett Azure Search index med hello REST-API, som du tänker utfärda en enkel HTTP POST-begäran tooyour Azure Search tjänstens URL-slutpunkt. Din indexdefinitionen ska ingå i hello begärantext som giltig JSON-innehåll.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Identifiera Azure Search-tjänstens API-administratörsnyckel
Nu när du har etablerat en Azure Search-tjänst kan du utfärda HTTP-begäranden mot din tjänst-URL-slutpunkten med hjälp av hello REST API. *Alla* API-förfrågningar måste innehålla hello api-nyckel som har genererats för hello söktjänsten du etablerat. Med en giltig nyckel upprättar förtroende mellan hello programmet skickar hello begäran och hello-tjänsten som hanterar den fall per begäran.

1. toofind din tjänst api-nycklar som du måste logga in på hello [Azure-portalen](https://portal.azure.com/)
2. Gå tooyour Azure Search service-bladet
3. Klicka på hello ”nycklar”-ikon

Tjänsten har *administratörsnycklar* och *frågenycklar*.

* Din primära och sekundära *admin nycklar* bevilja fullständiga rättigheter tooall åtgärder, inklusive hello möjlighet toomanage hello service, skapa och ta bort index, indexerare och datakällor. Det finns två nycklar så att du kan fortsätta toouse hello sekundärnyckeln om du väljer tooregenerate hello primärnyckel och vice versa.
* Din *fråga nycklar* bevilja läsbehörighet tooindexes och dokument och är vanligtvis distribuerade tooclient program som utfärdar search-begäranden.

För hello syftet med att skapa ett index, kan du använda antingen primär eller sekundär administrationsnyckel.

## <a name="define-your-azure-search-index-using-well-formed-json"></a>Definiera ditt Azure Search-index med välstrukturerad JSON
En enkel HTTP POST-begäran tooyour tjänst skapar ditt index. hello brödtext HTTP POST-begäran innehåller ett enda JSON-objekt som definierar ditt Azure Search-index.

1. första hello-egenskapen för den här JSON-objekt är hello namnet på ditt index.
2. andra hello-egenskapen för den här JSON-objekt är en JSON-matris med namnet `fields` som innehåller ett separat JSON-objekt för varje fält i ditt index. Var och en av dessa JSON-objekt innehåller flera namn/värde-par för varje hello fältattribut, bland annat ”name”, ”typ”, osv.

Det är viktigt att du behåller dina Sök upplevelse och företag behov i åtanke när du skapar ditt index som varje fält måste tilldelas hello [rätt attribut](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Dessa attribut styra vilka söka funktioner (filtrering, faceting, sortering fulltextsökning osv) gäller toowhich fält. För alla attribut som du inte anger blir hello standard tooenable hello motsvarande sökfunktionen såvida du inaktivera den.

I vårt exempel har vi gett indexet namnet ”hotels” och definierat fälten så här:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Vi har noggrant valt hello indexattribut för varje fält baserat på hur vi tror att de ska användas i ett program. Till exempel `hotelId` är en unik nyckel som användare söker efter hotell förmodligen inte vet, så vi inaktivera fulltextsökning för fältet genom att ange `searchable` för`false`, vilket sparar utrymme i hello index.

Observera att ett fält i ditt index av typen `Edm.String` hello betecknas som hello '' nyckelfält.

hello indexdefinitionen ovan använder ett språk analyzer för hello `description_fr` eftersom den är avsedd toostore franska text. Se [hello Language support avsnittet](https://docs.microsoft.com/rest/api/searchservice/Language-support) samt hello motsvarande [blogginlägget](https://azure.microsoft.com/blog/language-support-in-azure-search/) mer information om språkanalys.

## <a name="issue-hello-http-request"></a>Problemet hello HTTP-begäran
1. Med din indexdefinitionen som hello begärantext, utfärda en HTTP POST-begäran tooyour Azure Search-tjänsten slutpunkts-URL. I hello URL och vara säker på att toouse tjänstnamnet som hello värdnamn och placera hello rätt `api-version` som en frågesträngsparameter (hello aktuella API-versionen är `2016-09-01` när hello publicera det här dokumentet).
2. Ange hello i hello begärandehuvuden `Content-Type` som `application/json`. Du måste också tooprovide administrationsnyckeln för din tjänst som du identifierade i steg I i hello `api-key` huvud.

Du måste tooprovide egna namn och api nycklar tooissue hello tjänstbegäran nedan:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [api-key]


Statuskoden 201 (har skapats) bör returneras om begäran lyckades. Mer information om hur du skapar ett index via hello REST-API finns hello [här API-referens](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Mer information om andra HTTP-statuskoder som kan returneras om det uppstår fel finns i [HTTP-statuskoder (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

När du är klar med ett index och vill toodelete den bara utfärda en ta bort HTTP-begäran. Detta är till exempel hur vi skulle ta bort hello ”hotell” index:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2016-09-01
    api-key: [api-key]


## <a name="next-steps"></a>Nästa steg
När du har skapat ett Azure Search-index, kommer du att redo för[överföra innehållet till hello index](search-what-is-data-import.md) så att du kan börja söka data.
