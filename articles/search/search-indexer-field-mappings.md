---
title: "Fältet avbildningar i Azure Search indexerare"
description: "Konfigurera Azure Search indexeraren fältet diagrammen efter skillnader mellan fältnamn och representationer av data"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 0325a4de-0190-4dd5-a64d-4e56601d973b
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: eugenesh
ms.openlocfilehash: 57e91f070d9a42882a56e708f12b1ce238ed9191
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="field-mappings-in-azure-search-indexers"></a>Fältet avbildningar i Azure Search indexerare
När du använder Azure Search indexerare hittar ibland du själv i situationer där dina indata ganska inte matchar schemat för mål-index. I sådana fall kan du använda **fältet mappningar** att omvandla dina data till önskad form.

Vissa situationer där fältmappningar är användbara:

* Datakällan har ett fält `_id`, men Azure Search tillåter inte fältnamn som börjar med ett understreck. En avbildning kan du ”Byt namn på” ett fält.
* Vill du fylla i flera fält i indexet med samma data källdata, till exempel att du vill tillämpa olika analyzers till dessa fält. Fältmappningar kan du ”duplicera” ett fält i datakällan.
* Du måste till Base64 koda eller avkoda dina data. Fältmappningar stöd för flera **mappning funktioner**, inklusive funktioner för Base64-kodning och avkodning.   

## <a name="setting-up-field-mappings"></a>Ställa in fältmappningar
Du kan lägga till fältmappningar när du skapar ett nytt indexerare med det [skapa indexeraren](https://msdn.microsoft.com/library/azure/dn946899.aspx) API. Du kan hantera fältmappningar på en indexering indexerare med det [uppdatering indexeraren](https://msdn.microsoft.com/library/azure/dn946892.aspx) API.

En avbildning består av 3 delar:

1. En `sourceFieldName`, som motsvarar ett fält i datakällan. Den här egenskapen måste anges.
2. En valfri `targetFieldName`, som motsvarar ett fält i Sök-index. Om det utelämnas används samma namn som datakällan.
3. En valfri `mappingFunction`, vilket kan omvandla dina data med hjälp av ett av flera fördefinierade funktioner. En fullständig lista över funktioner är [under](#mappingFunctions).

Mappningar av fält har lagts till i den `fieldMappings` matrisen på indexeraren definition.

Här är exempelvis hur du kan hantera skillnader i fältnamn:

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
}
```

En indexerare kan ha flera fältmappningar. Till exempel här är hur du kan ”duplicera” ett fält:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" },
]
```

> [!NOTE]
> Azure Search använder skiftlägeskänsliga jämförelser för att matcha namnen på fältet och funktionen i fältet avbildningar. Det här är praktiskt (du behöver få alla gemener och versaler), men det innebär att din datakälla eller index inte kan ha fält som endast skiljer fallet.  
>
>

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Fältet mappning funktioner
Dessa funktioner stöds:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode"></a>base64Encode
Utför *URL-säkert* Base64-kodning av Indatasträngen. Förutsätter att UTF-8-kodade indata.

#### <a name="sample-use-case"></a>Använd exemplet
Endast URL-säkert tecken kan visas i en nyckel för Azure Search-dokument (eftersom kunder måste kunna adressera dokumentet med till exempel sökning-API). Om dina data innehåller URL-osäkra tecken och du vill använda den för att fylla i nyckelfältet i search-index, använder du den här funktionen.   

#### <a name="example"></a>Exempel
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Path",
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

<a name="base64DecodeFunction"></a>

### <a name="base64decode"></a>base64Decode
Utför Base64 avkodning av Indatasträngen. Indata antas ett *URL-säkert* Base64-kodad sträng.

#### <a name="sample-use-case"></a>Använd exemplet
BLOB anpassade metadatavärden måste vara ASCII-kodade. Du kan använda Base64-kodning för att representera godtycklig Unicode-strängar i blob anpassade metadata. För att göra sökningen beskrivande använda du dock den här funktionen för att göra kodade data tillbaka till ”vanlig” strängar vid påfyllning search-index.  

#### <a name="example"></a>Exempel
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" }
  }]
```

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition"></a>extractTokenAtPosition
Delar en string-fält med angiven avgränsare och hämtar token vid angiven position i den resulterande delningen.

Om indata är till exempel `Jane Doe`, `delimiter` är `" "`(blanksteg) och `position` är 0, resultatet är `Jane`; om den `position` är 1, resultatet är `Doe`. Om positionen refererar till en token som inte finns, returneras ett fel.

#### <a name="sample-use-case"></a>Använd exemplet
Datakällan innehåller en `PersonName` , och du vill indexera som två separata `FirstName` och `LastName` fält. Du kan använda den här funktionen för att dela indata med blanksteg som avgränsare.

#### <a name="parameters"></a>Parametrar
* `delimiter`: en sträng som ska användas som avgränsare när du delar upp Indatasträngen.
* `position`: ett heltal nollbaserade plats i token för att välja när Indatasträngen delas.    

#### <a name="example"></a>Exempel
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } }
  },
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } }
  }]
```

<a name="jsonArrayToStringCollectionFunction"></a>

### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection
Omvandlar en sträng formaterad som en JSON-matris med strängar i en strängmatris som kan användas för att fylla i en `Collection(Edm.String)` i indexet.

Om strängen är till exempel `["red", "white", "blue"]`, sedan målfältet av typen `Collection(Edm.String)` fylls med tre värden `red`, `white` och `blue`. Ett fel returneras indatavärden som inte kan parsas som JSON-strängen matriser.

#### <a name="sample-use-case"></a>Använd exemplet
Azure SQL-databasen inte har en inbyggd datatyp som naturligt mappar till `Collection(Edm.String)` fält i Azure Search. Om du vill fylla i samlingen strängfält källdata som en strängmatris JSON-format och använda den här funktionen.

#### <a name="example"></a>Exempel
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```

## <a name="help-us-make-azure-search-better"></a>Hjälp oss att förbättra Azure Search
Om du har funktionsförfrågningar om eller förslag på förbättringar kan du nå oss på vår [UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search/).
