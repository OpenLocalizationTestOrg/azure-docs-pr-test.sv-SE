---
title: aaaField avbildningar i Azure Search indexerare
description: "Konfigurera Azure Search indexeraren fältet mappningar tooaccount för skillnader i fältet namn och data garantier"
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
ms.openlocfilehash: 009d5dbc12cb9e8d9cfd3e8042e907ca88399ad7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="field-mappings-in-azure-search-indexers"></a>Fältet avbildningar i Azure Search indexerare
När du använder Azure Search indexerare hittar ibland du själv i situationer där dina indata ganska inte matchar hello schemat för mål-index. I sådana fall kan du använda **fältet mappningar** tootransform data till hello önskad form.

Vissa situationer där fältmappningar är användbara:

* Datakällan har ett fält `_id`, men Azure Search tillåter inte fältnamn som börjar med ett understreck. En avbildning kan du för ”ändra” ett fältnamn.
* Du vill toopopulate flera fält i hello index med hello samma datakällan data, till exempel eftersom du vill tooapply olika analyzers toothose fält. Fältmappningar kan du ”duplicera” ett fält i datakällan.
* Du behöver tooBase64 koda eller avkoda dina data. Fältmappningar stöd för flera **mappning funktioner**, inklusive funktioner för Base64-kodning och avkodning.   

## <a name="setting-up-field-mappings"></a>Ställa in fältmappningar
Du kan lägga till fältmappningar när du skapar en ny indexeraren med hello [skapa indexeraren](https://msdn.microsoft.com/library/azure/dn946899.aspx) API. Du kan hantera fältmappningar på en indexering indexerare med hello [uppdatering indexeraren](https://msdn.microsoft.com/library/azure/dn946892.aspx) API.

En avbildning består av 3 delar:

1. En `sourceFieldName`, som motsvarar ett fält i datakällan. Den här egenskapen måste anges.
2. En valfri `targetFieldName`, som motsvarar ett fält i Sök-index. Om det utelämnas används används hello samma namn som hello datakällan.
3. En valfri `mappingFunction`, vilket kan omvandla dina data med hjälp av ett av flera fördefinierade funktioner. hello fullständig lista över funktioner är [under](#mappingFunctions).

Mappningar av fälten läggs toohello `fieldMappings` matrisen på hello indexeraren definition.

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
> Azure Search använder skiftlägeskänsliga jämförelser tooresolve hello fältet och funktionen namn i fältet avbildningar. Det här är praktiskt (du inte tooget alla hello skiftläge höger), men det innebär att din datakälla eller index inte kan ha fält som endast skiljer fallet.  
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
Utför *URL-säkert* Base64-kodning av hello Indatasträngen. Förutsätter att UTF-8-kodat hello indata.

#### <a name="sample-use-case"></a>Använd exemplet
URL-säkert tecken kan bara visas i en nyckel för Azure Search-dokument (eftersom kunder måste vara kan tooaddress hello dokument med hello sökning API, till exempel). Om dina data innehåller URL-osäkra tecken och du vill toouse den toopopulate nyckelfältet i din sökindex, Använd den här funktionen.   

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
Utför Base64 avkodning av hello Indatasträngen. hello indata antas tooa *URL-säkert* Base64-kodad sträng.

#### <a name="sample-use-case"></a>Använd exemplet
BLOB anpassade metadatavärden måste vara ASCII-kodade. Du kan använda Base64-kodning toorepresent godtycklig Unicode-strängar i blob anpassade metadata. Dock toomake sökning beskrivande, du kan använda den här funktionen tooturn hello-kodade data tillbaka till ”vanlig” strängar vid påfyllning search-index.  

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
Dela ett string-fält med hello angivna avgränsare och plockningar hello token på hello angiven position i hello resulterande delning.

Till exempel om hello-indata är `Jane Doe`, hello `delimiter` är `" "`(blanksteg) och hello `position` är 0, hello resultatet är `Jane`; om hello `position` är 1, hello resultatet är `Doe`. Om hello position refererar tooa token som inte finns, returneras ett fel.

#### <a name="sample-use-case"></a>Använd exemplet
Datakällan innehåller en `PersonName` , och du vill tooindex det som två separata `FirstName` och `LastName` fält. Du kan använda den här funktionen toosplit hello som indata med hello blanksteg som hello avgränsare.

#### <a name="parameters"></a>Parametrar
* `delimiter`: en sträng toouse som hello avgränsare när dela hello Indatasträngen.
* `position`: ett heltal nollbaserade plats i hello token toopick när hello Indatasträngen delas.    

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
Transformeringar som en sträng formaterad som en JSON-matris med strängar i en strängmatris som kan använda toopopulate en `Collection(Edm.String)` i hello index.

Till exempel om hello Indatasträngen är `["red", "white", "blue"]`, och sedan hello målfältet av typen `Collection(Edm.String)` fylls med hello tre värden `red`, `white` och `blue`. Ett fel returneras indatavärden som inte kan parsas som JSON-strängen matriser.

#### <a name="sample-use-case"></a>Använd exemplet
Azure SQL-databasen inte har en inbyggd datatyp som naturligt mappar för`Collection(Edm.String)` fält i Azure Search. toopopulate sträng samling fält, källdata som en strängmatris JSON-format och använda den här funktionen.

#### <a name="example"></a>Exempel
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```

## <a name="help-us-make-azure-search-better"></a>Hjälp oss att förbättra Azure Search
Om du har funktionsförfrågningar om eller förslag på förbättringar kan du nå toous på vår [UserVoice-webbplatsen](https://feedback.azure.com/forums/263029-azure-search/).
