---
title: "aaaAzure Sök Service REST API-Version 2015-02-28-Preview | Microsoft Docs"
description: "Azure Search Service REST API-Version 2015-02-28-Preview innehåller experiment funktioner, till exempel naturliga språkanalys och moreLikeThis sökningar."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: 63cb30b7f43a42552c6744ea087afea947857224
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure Söktjänsts-REST API: Version 2015-02-28-Preview
Den här artikeln är hello i referensdokumentationen för `api-version=2015-02-28-Preview`. Den här förhandsgranskningen utökar hello aktuella allmänt tillgänglig version, [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), genom att tillhandahålla hello följande experiment funktioner:

* `moreLikeThis`Frågeparametern i hello [Sök dokument](#SearchDocs) API. Andra dokument som är relevanta tooanother specifikt dokument hittas.

Några ytterligare delar av hello `2015-02-28-Preview` REST API dokumenteras separat. Exempel på dessa är:

* [Bedömningsprofil profiler](search-api-scoring-profiles-2015-02-28-preview.md)
* [Indexerare](search-api-indexers-2015-02-28-preview.md)

Azure Search-tjänsten är tillgänglig i flera versioner. Se för[Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) mer information.

## <a name="apis-in-this-document"></a>API: er i det här dokumentet
API för Azure Search-tjänsten stöder två URL-syntax för API: et: enkelt och OData (se [stöd för OData (Azure Search-API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) information). hello visar följande lista hello enkel syntax.

[Skapa Index](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Uppdatera Index](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Hämta Index](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Visar en lista över index](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Få indexstatistik](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Testa Analyzer](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Ta bort ett Index](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Lägga till, ta bort, och uppdatera Data i ett Index](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Sök igenom dokument](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Sökning dokumentet](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Antal dokument](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Förslag](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a>Indexåtgärder
Du kan skapa och hantera index i Azure Search-tjänsten via enkel HTTP-begäranden (POST, GET, PUT, DELETE) mot en resurs med angivet index. toocreate ett index du bokför först JSON-dokument som beskriver hello indexeringsschema. hello schema definierar hello fält hello index, deras datatyper och hur de kan användas (t.ex, i fulltextsökningar, filter, sortering eller faceting). Den definierar även bedömningsprofil profiler, suggesters och andra attribut tooconfigure hello beteendet för hello index.

hello innehåller följande exempel en illustration av ett schema som används för att söka på hotellinformation med hello beskrivningsfält som definierats i två språk. Observera hur attribut styr hur hello fältet används. Till exempel hello `hotelId` används som hello Dokumentnyckel (`"key": true`) och har exkluderats från fulltextsökningar (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

När hello indexet har skapats måste du överföra dokument som fyller på hello index. Se [Lägg till eller uppdatera dokument](#AddOrUpdateDocuments) för nästa steg.

En videopresentation tooindexing i Azure Search finns hello [Channel 9 molnet omfattar avsnitt på Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).

<a name="CreateIndex"></a>

## <a name="create-index"></a>Skapa index
Ett index är hello primära sätt att ordna och söka dokument i Azure Search, liknande toohow en tabell ordnar poster i en databas. Varje indexet innehåller en samling dokument att alla kraven toohello indexeringsschema (fältnamn, datatyper och egenskaper), men index också ange ytterligare konstruktioner (suggesters, bedömningsprofil profiler och alternativ för CORS) som definierar andra Sök beteenden.

Du kan skapa ett nytt index i en Azure Search-tjänst med en HTTP POST eller PUT-begäran. hello brödtext hello-begäran är en JSON-schema som innehåller information om hello index och konfiguration.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Du kan också använda PUT och ange hello Indexnamnet på hello URI. Om hello indexet inte finns, kommer att skapas.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Skapa ett index anger hello strukturen för hello dokument lagras och används i sökningar. Fylla hello index är en separat åtgärd. Det här steget kan du använda en [indexeraren](https://msdn.microsoft.com/library/azure/mt183328.aspx) (tillgängligt för datakällor som stöds) eller en [Lägg till, uppdatera eller ta bort dokument](https://msdn.microsoft.com/library/azure/dn798930.aspx) igen. hello inverterad index skapas vid hello dokument.

**Obs**: hello maximalt antal tillåtna index varierar beroende på prisnivå. hello ledigt tjänsten kan in too3 index. Standard-tjänsten tillåter 50 index per söktjänst. Se [gränser och begränsningar](http://msdn.microsoft.com/library/azure/dn798934.aspx) mer information.

**Förfrågan**

HTTPS krävs för alla tjänstbegäranden. Hej **Create Index** begäran kan konstrueras med en POST eller PUT metod. När du använder POST, måste du ange ett indexnamn i begärandetexten för hello tillsammans med hello index schemadefinition. Med PUT är hello Indexnamnet en del av hello-URL. Om det inte finns hello index skapas. Om det redan finns är uppdaterade toohello ny definition.

hello Indexnamn måste vara gemener, börja med en bokstav eller siffra, har inga snedstreck eller punkter och vara mindre än 128 tecken. När du har startat hello Indexnamnet med en bokstav eller siffra kan hello resten av hello namn innehålla en bokstav, antal och tankstreck, så länge hello streck inte är i följd.

Hej `api-version` krävs. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) för en lista över tillgängliga versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `Content-Type`: Krävs. Ställ in för`application/json`
* `api-key`: Krävs. Hej `api-key` används för att
* autentisera hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **Create Index** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello service name tooconstruct hello begärd URL. Du får båda hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

<a name="RequestData"></a>
**Begäran brödtext Syntax**

hello hello-begäran innehåller en schemadefinition som innehåller hello lista över datafält i dokument som kan matas in i detta index, datatyper, attribut, samt en valfri lista med bedömningen profiler som är används tooscore matchar dokument på Frågetid.

Observera att för en POST-begäran måste du ange hello Indexnamnet i hello begärandetexten.

Det kan bara finnas ett nyckelfält i hello index. Toobe har ett strängfält. Det här fältet motsvarar hello unika identifieraren för varje dokument som lagras i hello index.

hello delar av ett index inkludera hello följande:

* `name`
* `fields`som kan matas in i detta index, inklusive namn, datatyp och egenskaper som definierar tillåtna åtgärder på fältet.
* `suggesters`används för automatisk komplettering eller typ-ahead-frågor.
* `scoringProfiles`används för anpassad sökning poäng rangordning. Se [Lägg till bedömningsprofil profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) mer information.
* `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` används toodefine hur dina dokument/frågor är uppdelade indexeras/sökbara token. Se [analys i Azure Search](https://aka.ms//azsanalysis) mer information.
* `defaultScoringProfile`använda toooverwrite hello standard bedömningen beteenden.
* `corsOptions`tooallow cross-origin frågor mot ditt index.

hello-syntax för att strukturera hello nyttolasten i begäran är som följer. Ett exempel på en begäran har angetts mer på i det här avsnittet.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indexattribut**

hello kan följande attribut anges när ett index skapas. Mer information om batchbedömning och bedömningen profiler finns [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Aktiverar hello namnet på hello fält.

`type`-Aktiverar hello datatyp för hello fält.

`searchable`-Markerar hello fält som fulltext-sökning kan. Det innebär att den kommer att göras analys, till exempel radbrytning under indexeringen. Om du ställer in en `searchable` tooa fältvärdet som ”skiner”, internt blir dela till hello enskilda token ”Soligt” och ”dag”. Detta gör att fulltextsökningar för dessa villkor. Fält av typen `Edm.String` eller `Collection(Edm.String)` är `searchable` som standard. Fält av andra typer kan inte vara `searchable`.

* **Obs**: `searchable` fält använda extra utrymme i ditt index eftersom en ytterligare principfilerna version av hello fältvärdet fulltextsökningar lagras i Azure Search. Om du vill toosave utrymme i ditt index och du behöver inte en fältet toobe som ingår i sökningar genom att ange `searchable` för`false`.

`filterable`-Tillåter hello fältet toobe som refereras i `$filter` frågor. `filterable`skiljer sig från `searchable` i hur strängar ska hanteras. Fält av typen `Edm.String` eller `Collection(Edm.String)` som är `filterable` inte genomgår radbrytning, så att jämförelser för endast exakt matchar. Till exempel om du ställer in sådant fält `f` för ”skiner” `$filter=f eq 'sunny'` hittar inga matchningar men `$filter=f eq 'sunny day'` kommer. Alla fält är `filterable` som standard.

`sortable`-Som standard sorterar hello system resultat poäng, men i många upplevelser användare vill toosort av fälten i hello dokument. Fält av typen `Collection(Edm.String)` får inte vara `sortable`. Alla andra fält är `sortable` som standard.

`facetable`-Vanligtvis används i en presentation av sökresultat som innehåller antalet träffar per kategori (till exempel, Sök efter digitalkameror och se träffar efter varumärke, megapixel, av pris, etc.). Det här alternativet kan inte användas med fält av typen `Edm.GeographyPoint`. Alla andra fält är `facetable` som standard.

* **Obs**: fält av typen `Edm.String` som är `filterable`, `sortable`, eller `facetable` kan vara högst 32 KB längd. Detta beror på att dessa fält behandlas som en enda sökterm och hello maxlängd för en term i Azure Search är 32KB. Om du behöver toostore mer text än detta i ett enda strängfält måste tooexplicitly ange `filterable`, `sortable`, och `facetable` för`false` i Indexdefinition.
* **Obs**: om ett fält har inget av ovanstående hello attributen för angivna`true` (`searchable`, `filterable`, `sortable`, eller`facetable`) hello fältet effektivt är exkluderad från hello inverterad index. Det här alternativet är användbart för fält som inte används i frågor, men som behövs i sökresultaten. Förutom dessa fält från hello indexet förbättrar prestanda.

`key`-Märken hello fältet innehåller unika identifierare för dokument inom hello index. Exakt ett fält måste väljas som hello `key` fältet och det måste vara av typen `Edm.String`. Nyckelfält kan vara används toolook dokument direkt via hello [sökning API](#LookupAPI).

`retrievable`-Anger om fältet hello kan returneras i sökresultatet.  Detta är användbart när du vill toouse ett fält (till exempel marginal) som ett filter som sortering eller bedömningen mekanism men inte vill att hello fältet toobe synliga toohello slutanvändaren. Det här attributet måste vara `true` för `key` fält.

`analyzer`-Aktiverar hello namnet på hello analyzer toouse för hello fält vid sökning och indexering tid. Hello tillåtna värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Det här alternativet kan användas med `searchable` fält och den kan inte anges tillsammans med antingen `searchAnalyzer` eller `indexAnalyzer`.  När hello analyzer är valt, kan inte ändras för hello-fältet.

`searchAnalyzer`-Aktiverar hello hello analyzer används för närvarande sökning för hello fältet namn. Hello tillåtna värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Det här alternativet kan användas med `searchable` fält. Det måste anges tillsammans med `indexAnalyzer` och kan inte anges tillsammans med hello `analyzer` alternativet. Den här analyzer kan uppdateras på ett befintligt fält.

`indexAnalyzer`-Aktiverar hello hello analyzer används för närvarande indexering för hello fältet namn. Hello tillåtna värden finns [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Det här alternativet kan användas med `searchable` fält. Det måste anges tillsammans med `searchAnalyzer` och kan inte anges tillsammans med hello `analyzer` alternativet. När hello analyzer är valt, kan inte ändras för hello-fältet.

`suggesters`-Aktiverar hello sökläget och fält som är hello informationskälla hello förslag. Se [Suggesters](#Suggesters) mer information.

`scoringProfiles`-Definierar anpassade bedömningsprofil beteenden som gör att du kan påverka vilka objekt som visas överst i sökresultaten. Bedömningsprofil profiler består av fältvikter och funktioner. Se [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information om hello-attribut som används i en bedömningsprofilen.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Språkstöd**

Sökbara fält genomgå analys som omfattar ofta avstavning text normalisering och filtrera bort villkor. Som standard sökbara fält i Azure Search analyseras med hello [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) som delar upp text i element som följer den[”Unicode-Text segmentering”](http://unicode.org/reports/tr29/) regler. Dessutom konverterar hello standard analyzer alla tecken tootheir gemen formuläret. Både indexerade dokument och sökvillkoren kan du gå igenom hello analys under indexering och frågebearbetning.

Azure Search har stöd för flera olika språk. Varje språk kräver en icke-standard text analyzer som-konton för egenskaperna för ett visst språk. Azure Search finns två typer av analyzers:

* 35 analyzers backas upp av Lucene.
* 50 analyzers backas upp av egna Microsoft naturligt språk bearbetning teknik som används i Office och Bing.

Vissa utvecklare kanske föredrar hello mer bekanta, enkla, öppen källkod lösning av Lucene. Lucene analyzers är snabbare, men hello Microsoft analyzers har avancerade funktioner, t.ex lemmatisering, word decompounding (i språk som tyska, danska, nederländska, svenska, norska, estniska, Slutför, ungerska, slovakiska) och entitet recognition (URL: er e-postmeddelanden, datum, nummer). Om möjligt bör du köra jämförelser av båda hello Microsoft och Lucene analyzers toodecide vilket som är en bättre anpassning.

***Jämförelse***

Hej Lucene analyzer för engelska utökar hello standard analyzer. Det tar bort genitiv (avslutande) från ord, gäller härrör enligt [Porter härrör algoritmen](http://tartarus.org/~martin/PorterStemmer/), och tar bort engelska [stoppord](http://en.wikipedia.org/wiki/Stop_words).

Jämförelse utför hello Microsoft analyzer lemmatisering i stället för härrör. Det innebär att den kan hantera böjda och oregelbundna word formulär förbättras vad resulterar i mer relevant sökresultat (titta på modul 7 av [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) för mer information).

Indexering med Microsoft analyzers är ungefär två toothree gånger långsammare än motsvarigheterna Lucene beroende på hello språk. Sökningsprestanda avsevärt påverkas inte för frågor genomsnittlig storlek.

***Konfiguration***

Du kan ange hello för varje fält i hello indexdefinitionen `analyzer` tooan analyzer egenskapsnamn som anger vilka språk och leverantör. hello samma analyzer tillämpas när indexering och sökning för fältet.
Du kan till exempel ha separata fält för engelska, franska och spanska hotell beskrivningar som finns sida vid sida i hello samma index. Använd hello ['searchFields' Frågeparametern](#SearchQueryParameters) toospecify vilka språkspecifika fältet toosearch mot i dina frågor. Du kan granska frågan exempel som omfattar hello `analyzer` egenskap i [Sök dokument](#SearchDocs). 

***Analyzer lista***

Nedan visas hello lista över språk som stöds tillsammans med Lucene och Microsoft analyzer namn.

<table style="font-size:12">
    <tr>
        <th>Språk</th>
        <th>Microsoft analyzer namn</th>
        <th>Lucene analyzer namn</th>
    </tr>
    <tr>
        <td>Arabiska</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>        
    </tr>
    <tr>
        <td>Armeniska</td>
        <td></td>
        <td>hy.lucene</td>
      </tr>
    <tr>
        <td>Bangla</td>
        <td>bn.Microsoft</td>
        <td></td>
    </tr>
      <tr>
        <td>Baskiska</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
      <tr>
         <td>Bulgariska</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
      </tr>
      <tr>
        <td>Katalanska</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
      </tr>
    <tr>
        <td>Kinesiska, förenklad</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>        
    </tr>
    <tr>
        <td>Kinesiska, traditionell</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>        
    <tr>
    <tr>
        <td>Kroatiska</td>
        <td>HR.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Tjeckiska</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>        
    </tr>    
    <tr>
        <td>Danska</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>        
    </tr>    
    <tr>
        <td>Nederländska</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>    
    </tr>    
    <tr>
        <td>Svenska</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>        
    </tr>
    <tr>
        <td>Estniska</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Finska</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>        
    </tr>    
    <tr>
        <td>Franska</td>
        <td>fr.Microsoft</td>
        <td>fr.lucene</td>        
    </tr>
    <tr>
        <td>Galiciska</td>
        <td></td>
        <td>GL.lucene</td>        
      </tr>
    <tr>
        <td>Tyska</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>        
    </tr>
    <tr>
        <td>Grekiska</td>
        <td>el.Microsoft</td>
        <td>el.lucene</td>        
    </tr>
    <tr>
        <td>Gujarati</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebreiska</td>
        <td>HE.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>        
    </tr>
    <tr>
        <td>Ungerska</td>        
        <td>HU.Microsoft</td>
        <td>HU.lucene</td>
    </tr>
    <tr>
        <td>Isländska</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonesiska (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>        
    </tr>
    <tr>
        <td>Irländska</td>
        <td></td>
          <td>GA.lucene</td>
    </tr>
    <tr>
        <td>Italienska</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>        
    </tr>
    <tr>
        <td>Japanska</td>
        <td>Ja.Microsoft</td>
        <td>Ja.lucene</td>

    </tr>
    <tr>
        <td>Kannada</td>
        <td>KA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Koreanska</td>
        <td>Ko.Microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Lettiska</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>    
    </tr>
    <tr>
        <td>Litauiska</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malayalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajiska (latinsk)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>MR.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norska</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>        
    </tr>
      <tr>
        <td>Persiska</td>
        <td></td>
        <td>fa.lucene</td>        
      </tr>
    <tr>
        <td>Polska</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>        
    </tr>
    <tr>
        <td>Portugisiska (Brasilien)</td>
        <td>pt-Br.microsoft</td>
        <td>pt-Br.lucene</td>        
    </tr>
    <tr>
        <td>Portugisiska (Portugal)</td>
        <td>pt-Pt.microsoft</td>        
        <td>pt-Pt.lucene</td>
    </tr>
    <tr>
        <td>Punjabi</td>
        <td>pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Rumänska</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Ryska</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>    
    </tr>
    <tr>
        <td>Serbiska (kyrillisk)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Serbiska (latinsk)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovakiska</td>
        <td>Sk.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovenska</td>
        <td>Sl.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Spanska</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>Svenska</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>Tamilska</td>
        <td>ta.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thai</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>Turkiska</td>
        <td>TR.Microsoft</td>
        <td>TR.lucene</td>        
    </tr>
    <tr>
        <td>Ukrainska</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamesiska</td>
        <td>Vi.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Dessutom ger Azure Search språkoberoende analyzer konfigurationer</td>
    <tr>
        <td>Standard-ASCII vikning</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode-text segmentering (Standard Tokenizer)</li>
            <li>Vikning filtret för ASCII - omvandlar Unicode-tecken som inte tillhör toohello uppsättning först 127 ASCII-tecken till deras motsvarigheter i ASCII. Detta är användbart för att ta bort diakritiska tecken.</li>
        </ul>
        </td>
    </tr>
</table>

Alla analyzers med namn som är försedd med <i>lucene</i> drivs av [Apache Lucene språkanalys](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Mer information om hello ASCII vikning filtret hittar [här](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Förslag på alternativ**

En `suggester` definierar vilka fält i ett index som används toosupport automatisk komplettering i sökningar. Vanligtvis partiella söksträngar skickas toohello [förslag API](#Suggestions) medan hello användaren skriver en sökfråga och hello API returnerar en uppsättning föreslagna fraser. En förslagsställarens som du definierar i hello index anger vilka fält som används toobuild hello typ-ahead sökvillkoren. Se [Suggesters](#Suggesters) konfigurationsinformation.

**Poängprofiler**

En `scoringProfile` definierar anpassade bedömningsprofil beteenden som gör att du kan påverka vilka objekt som visas högre upp i hello sökresultat. Bedömningsprofil profiler består av fältvikter och funktioner. toouse dem du anger en profil med namnet på hello frågesträngen.

Standard bedömningen profil verkar bakom hello i bakgrunden toocompute Sök poängen för varje objekt i en resultatuppsättning. Du kan använda hello interna, namnlösa bedömningsprofilen. Du kan också ange `defaultScoringProfile` toouse en anpassad profil som hello standard anropas när en anpassad profil inte har angetts på hello frågesträngen.

Se [Lägg till bedömningen profiler tooa sökindex (Azure Söktjänsts-REST API)](search-api-scoring-profiles-2015-02-28-preview.md) mer information.

**Alternativ för CORS**

Javascript för klientsidan kan inte anropa alla API: er som standard eftersom hello webbläsaren hindrar alla cross-origin-begäranden. Aktivera CORS (Cross-Origin Resource Sharing) genom att ange hello `corsOptions` attributet tooallow cross-origin frågor tooyour index. Observera att endast query API: er stöd CORS av säkerhetsskäl. hello följande alternativ kan anges för CORS:

* `allowedOrigins`(krävs): det här är en lista över ursprung som beviljas åtkomst tooyour index. Detta innebär att all Javascript-kod från dessa ursprung blir tillåtna tooquery ditt index (förutsatt att den ger hello rätt API-nyckel). Varje ursprung har vanligtvis hello formuläret `protocol://fully-qualified-domain-name:port` även om hello port utelämnas ofta. Se [i den här artikeln](http://go.microsoft.com/fwlink/?LinkId=330822) för mer information.
  * Även om du vill tooallow åtkomst tooall ursprung `*` som ett enskilt objekt i hello `allowedOrigins` matris. Observera att **detta rekommenderas inte för produktion search-tjänster.** Det kan dock vara användbar för utveckling eller felsökning.
* `maxAgeInSeconds`(valfritt): webbläsare använder det här värdet toodetermine hello (i sekunder) toocache CORS preflight-svar. Detta måste vara ett icke-negativt heltal. hello större det här värdet är hello bättre prestanda, men hello längre tid det tar för CORS princip ändringar tootake effekt. Om den inte är inställd kommer på 5 minuter att används standardvärdet.

<a name="CreateUpdateIndexExample"></a>
**Begäran Body-exempel**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Svar**

För en lyckad begäran: ”201 skapad”.

Som standard innehåller hello svarstexten hello JSON för hello indexdefinitionen som har skapats. Om hello `Prefer` begärandehuvudet har angetts för`return=minimal`hello svarstexten är tomt och hello lyckade statuskoden ska vara ”204 saknar innehåll” i stället för ”201 skapad”. Detta gäller oavsett om PUT eller POST har använt toocreate hello index.

**Kommentarer**

Det finns för närvarande begränsat stöd för index schema-uppdateringar. Schema-uppdateringar som kräver omindexering, till exempel ändring av fälttyp stöds inte för närvarande. Även om befintliga fält inte kan ändras eller tas bort kan läggas nya fält tooan befintligt index när som helst. När ett nytt fält läggs alla befintliga dokument i hello index automatiskt att ha ett null-värde för fältet. Inga ytterligare lagringsutrymme används förrän nya dokument har lagts till toohello index.

<a name="Suggesters"></a>

## <a name="suggesters"></a>Förslag på alternativ
hello förslag funktion i Azure Search är en typ i förväg eller automatisk komplettering frågan-funktion som ger en lista över möjliga sökorden i svar toopartial sträng indata som angetts i Sök-rutan. Har du antagligen märkt frågeförslag när du använder kommersiella sökmotorer: att skriva ”.NET” i Bing genererar en lista över villkor för ”.NET 4.5” ”, .NET Framework 3,5-tums, och så vidare. När du använder hello söktjänsten REST-API, kräver implementera förslag i ett anpassat program för Azure Search hello följande:

* Aktivera förslag genom att lägga till en **förslagsställarens** konstruktionen i ditt index ger hello namn, sökläget och en lista med fält som typen ahead anropas. Till exempel om du anger ”cityName” som ett fält i datakällan att skriva en partiell sökning sträng med ”Sea” leder till att ”Seattle”, ”snäckor” och ”Seatac” (alla tre är faktiska stadsnamn) erbjuds som frågan förslag toohello användare.
* Anropa förslag genom att anropa hello [förslag API](#Suggestions) i programkoden. Vanligtvis en partiell söksträngar skickas toohello tjänsten medan hello användaren skriver en sökfråga och returnerar en uppsättning föreslagna fraser för detta API.

Den här artikeln förklarar hur tooconfigure en **förslagsställarens**. Du bör också granska hello [förslag API](#Suggestions) information om hur en förslagsställarens används.

**Användning**

`Suggesters`skapas i hello index och fungerar bäst när det används toosuggest specifika dokument i stället för att lösa ord eller fraser. hello bra kandidat fält är titlar, namn och andra relativt korta fraser som kan identifiera ett objekt. Mindre effektiva är återkommande fält, till exempel kategorier och taggar eller mycket långa fält, till exempel beskrivningar och kommentarer fält.

Som en del av hello indexdefinitionen, kan du lägga till en enda förslagsställarens toohello `suggesters` samling. Egenskaper som definierar en förslagsställarens inkludera hello följande:

* `name`: hello hello förslagsställarens namn. Du kan använda hello hello förslagsställarens namn när du anropar hello `suggest` API.
* `searchMode`: hello strategi som används för toosearch efter möjliga fraser. hello läge stöds för närvarande är `analyzingInfixMatching`, vilket genomför flexibla matchningar av fraser i början av hello eller hello mitten av meningarna.
* `sourceFields`: En lista över ett eller flera fält som är hello informationskälla hello förslag. Endast fält av typen `Edm.String` och `Collection(Edm.String)` kanske källor för förslag. Endast de fält som inte har en anpassad språk analyzer ange kan användas.

**Förslagsställarens-exempel**

En förslagsställarens är en del av hello index. Endast en förslagsställarens kan finnas i hello `suggesters` samling i hello aktuell version tillsammans med hello fält samlingen och `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> Om du använde hello offentliga förhandsversionen av Azure Search `suggesters` ersätter en äldre boolesk egenskap (`"suggestions": false`) som bara stöds prefix förslag för korta strängar (3-25 tecken). Dess ersättare `suggesters`, stöder infix matchar som söker efter matchande villkoren hello början eller mitten hello av fältet innehåll med bättre tolerans för fel i söksträngar. Från och med hello allmänt tillgänglig, är detta nu hello endast implementering av hello förslag API. hello äldre `suggestions` egenskap som introducerades i `api-version=2014-07-31-Preview` fortsätter toowork i den här versionen, men är inte funktionsduglig i hello `2015-02-28` eller senare versioner av Azure Search.
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a>Uppdatera Index
Du kan uppdatera ett befintligt index i Azure Search med hjälp av en HTTP PUT-begäran. Uppdateringar kan inkludera att lägga till nya fält toohello befintligt schema, ändra alternativ för CORS och ändrar bedömningsprofil profiler. Se [lägga till bedömningen profiler](https://msdn.microsoft.com/library/azure/dn798928.aspx) för mer information. Du kan ange hello namnet på hello index tooupdate på hello URI-begäran:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Viktigt:** stöd för index schemauppdateringar är begränsad toooperations som inte kräver återskapa hello sökindex. Schema-uppdateringar som kräver omindexering, till exempel ändring av fälttyp, stöds inte för närvarande. Nya fält läggas när som helst, även om befintliga fält inte kan ändras eller tas bort. hello samma sak gäller för`suggesters`. Går att lägga till nya fält tooa förslagsställarens på hello hello tidsfälten läggs men fält kan inte tas bort från `suggesters` och befintliga fält kan inte läggas till för`suggesters`.

När du lägger till ett nytt fält tooan index har alla befintliga dokument i hello index automatiskt ett null-värde för fältet. Inga ytterligare lagringsutrymme används förrän nya dokument har lagts till toohello index.

**Förfrågan**

HTTPS krävs för alla tjänstbegäranden. Hej **uppdatera Index** begäran har skapats med hjälp av HTTP PUT. Med PUT är hello Indexnamnet en del av hello-URL. Om det inte finns hello index skapas. Om det redan finns hello index är uppdaterade toohello ny definition.

hello Indexnamn måste vara gemener, börja med en bokstav eller siffra, har inga snedstreck eller punkter och vara mindre än 128 tecken. När du har startat hello Indexnamnet med en bokstav eller siffra kan hello resten av hello namn innehålla en bokstav, antal och tankstreck, så länge hello streck inte är i följd.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `Content-Type`: Krävs. Ställ in för`application/json`
* `api-key`: Krävs. Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **uppdatera Index** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begäran brödtext Syntax**

När du uppdaterar ett befintligt index, hello brödtext måste innehålla hello ursprungliga schemadefinition plus hello nya fält som du lägger till, samt hello ändras bedömningsprofil profiler, suggesters och alternativ för CORS, om sådana finns. Om du inte ändrar hello bedömningsprofil profiler och alternativ för CORS, måste du inkludera hello original från när hello indexet har skapats. I allmänhet hello bästa mönstret toouse för uppdateringar är tooretrieve hello indexdefinitionen med GET, ändra den och sedan uppdatera med PUT.

hello-schemasyntaxen används ett index återges här toocreate i informationssyfte. Se [Create Index](#CreateIndex) för mer information.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Svar**

För en lyckad begäran ”: 204 saknar innehåll”.

Som standard är hello svarstexten tom. Emellertid, om hello `Prefer` begärandehuvudet har angetts för`return=representation`, hello svarstexten innehåller hello JSON för hello indexdefinitionen som har uppdaterats. I det här fallet hello lyckade statuskoden ska vara ”200 OK”.

**Uppdaterar indexdefinitionen med anpassade analyzers**

När en analyzer, en tokenizer, ett token filter eller ett char-filter har definierats kan inte ändras. Nya kan läggas till tooan befintligt index om hello `allowIndexDowntime` flaggan anges tootrue i hello index uppdateringsbegäran: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Observera att den här åtgärden placerar ditt index offline för minst ett par sekunder, och din indexering och fråga begär toofail. Prestanda- och skrivbehörighet tillgängligheten för hello indexet kan vara försämrad i flera minuter när hello indexet uppdateras eller längre för mycket stora index.

<a name="ListIndexes"></a>

## <a name="list-indexes"></a>Lista över index
Hej **listan index** åtgärden returnerar en lista över hello index för närvarande i Azure Search-tjänsten.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Förfrågan**

HTTPS krävs för alla tjänstbegäranden. Hej **listan index** begäran kan konstrueras med hello GET-metoden.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `api-key`: Krävs. Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **listan index** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

Ingen.

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

Här är ett exempel svarstexten:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Observera att du kan filtrera hello svar ned toojust hello egenskaper som du är intresserad av. Till exempel om du vill att endast en lista över namn på index använda hello OData `$select` frågealternativet:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

I det här fallet visas hello svar från hello över exempel på följande sätt:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Det här är en bra metod toosave bandbredd om du har många index i din söktjänst.

<a name="GetIndex"></a>

## <a name="get-index"></a>Hämta Index
Hej **hämta Index** åtgärden hämtar hello indexdefinitionen från Azure Search.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Förfrågan**

HTTPS krävs för tjänstbegäranden. Hej **hämta Index** begäran kan konstrueras med hello GET-metoden.

Hej [index name] i hello Begärd URI anger vilka index tooreturn från hello GetSchema.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **hämta Index** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

Ingen.

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

Se hello exempel JSON i [att skapa och uppdatera ett Index](#CreateUpdateIndexExample) ett exempel på hello svar nyttolast.

<a name="DeleteIndex"></a>

## <a name="delete-index"></a>Ta bort indexet
Hej **ta bort indexet** åtgärden tar bort ett index och associerade dokument från din Azure Search-tjänst. Du kan hämta hello Indexnamnet från hello service instrumentpanelen i hello Azure-portalen, eller från hello API. Se [listan index](#ListIndexes) mer information.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Förfrågan**

HTTPS krävs för tjänstbegäranden. Hej **ta bort indexet** begäran kan konstrueras med hello DELETE-metoden.

Hej [index name] i hello Begärd URI anger vilka index toodelete från hello GetSchema.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `api-key`: Krävs. Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour tjänst-URL. Hej **ta bort indexet** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

Ingen.

**Svar**

Statuskod: 204 Nej innehåll returneras för ett lyckat svar.

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a>Få indexstatistik
Hej **hämta indexstatistik** åtgärd returnerar från Azure Search dokumentet antalet hello aktuellt index plus lagringskvoten.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Statistik om dokumentet antal och lagringsstorlek har samlats in några minuters mellanrum, inte i realtid. Hello statistik som returneras av denna API kan därför inte återspegla ändringar på grund av de senaste Indexeringsåtgärder.
> 
> 

**Förfrågan**

HTTPS krävs för alla begäranden som tjänster. Hej **hämta indexstatistik** begäran kan konstrueras med hello GET-metoden.

Hej [index name] i hello Begärd URI visar hello tooreturn indexstatistik för hello angivet index.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **hämta indexstatistik** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

Ingen.

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

hello svarstexten finns i hello följande format:

    {
      "documentCount": number,
      "storageSize": number (size of hello index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a>Testa Analyzer
Hej **analysera API** visar hur en analyzer delar upp text i token.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Förfrågan**

HTTPS krävs för alla begäranden som tjänster. Hej **analysera API** begäran kan konstrueras med hello POST-metoden.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **analysera API** begäran måste innehålla en `api-key` set tooan admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello Indexnamnet och hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

    {
      "text": "Text tooanalyze",
      "analyzer": "analyzer_name"
    }

eller

    {
      "text": "Text tooanalyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

Hej `analyzer_name`, `tokenizer_name`, `token_filter_name` och `char_filter_name` behöver toobe giltiga namn på fördefinierade eller anpassade analyzers, tokenizers, token-filter och char filter för hello index. toolearn om hello processen lexikala analys finns [analys i Azure Search](https://aka.ms/azsanalysis).

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

hello svarstexten finns i hello följande format:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of hello first character of hello token),
          "endOffset": number (index of hello last character of hello token),
          "position": number (position of hello token in hello input text)
        },
        ...
      ]
    }

**Analysera API-exempel**

**Förfrågan**

    {
      "text": "Text tooanalyze",
      "analyzer": "standard"
    }

**Svar**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a>Dokumentet åtgärder
I Azure Search index lagras i molnet hello och fylls i med hjälp av JSON-dokument som du överför toohello service. Alla hello-dokument som du överför utgör hello Kristi Sök data. Dokument innehåller fält, av vilka vissa tokeniserad i söktermer som de överförs. Hej `/docs` URL-segment i hello Azure Search API representerar hello samling av dokument i ett index. Alla åtgärder som utförts på hello samling, till exempel överföra sammanslagning, ta bort eller fråga dokument ske i hello kontexten för ett index, så hello URL: er för dessa åtgärder börjar alltid med `/indexes/[index name]/docs` efter ett angivet index.

Programkoden måste antingen generera JSON-dokument tooupload tooAzure Sök eller använda en [indexeraren](https://msdn.microsoft.com/library/dn946891.aspx) tooload dokument om datakällan hello Azure SQL Database eller Azure Cosmos DB. Normalt fylls index i från en enda datamängd som du anger.

Du bör planera ett dokument för varje objekt som du vill toosearch ska ha. En film uthyrning programmet kan ha ett dokument per film, en storefront programmet kan ha ett dokument per SKU, ett online kursprogramvara program kan ha ett dokument per kursen, research företag kan ha ett dokument för varje academic papper i sin databas och så vidare.

Dokument består av ett eller flera fält. Fälten kan innehålla text som är tokeniserad med Azure Search in söktermer som inte tokeniserad eller icke-textvärden som kan användas i filter eller bedömningsprofil profiler. hello bestäms namn, datatyper och funktioner som stöds för varje fält av hello indexeringsschema. Ett av hello fält i varje indexeringsschema måste anges som ett ID och varje dokument måste ha ett värde för hello-fält som unikt identifierar dokumentet i hello index. Alla andra dokumentfält är valfria och som standard tooa null-värde om lämnas Ospecificerad. Observera att null-värden inte plats i hello sökindex.

Innan du kan ladda upp dokument, måste du ha skapat hello index på hello-tjänsten. Se [Create Index](#CreateIndex) mer information om det här första steget.

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a>Lägga till, uppdatera, eller ta bort dokument
Du kan ladda upp, merge, merge eller överför eller ta bort dokument från ett specificerat index med hjälp av HTTP POST. För många uppdateringar rekommenderas massbearbetning av dokument (too1000 dokument per batch) eller ca 16 MB per batch.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Förfrågan**

HTTPS krävs för alla tjänstbegäranden. Du kan ladda upp, merge, merge eller överför eller ta bort dokument från ett specificerat index med hjälp av HTTP POST.

hello begäran URI innehåller [Indexnamnet] anger vilka index toopost dokument. Du kan bara publicera dokument tooone index i taget.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `Content-Type`: Krävs. Ställ in för`application/json`
* `api-key`: Krävs. Hej `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour service. Hej **lägga till dokument** begäran måste innehålla en `api-key` huvud ange tooyour admin-nyckel (som skillnad från tooa Frågenyckeln).

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

hello hello-begäran innehåller en eller flera dokument toobe indexeras. Dokument identifieras med en unik nyckel. Varje dokument som är associerat med en åtgärd: Överför dokument, mergeOrUpload eller ta bort. Överför förfrågningar måste innehålla hello dokumentdata som en uppsättning nyckel/värde-par.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> Dokumentet nycklar kan endast innehålla bokstäver, siffror, bindestreck (”-”), understreck (”_”) och likhetstecken (”=”). Mer information finns i [namngivningsregler](https://msdn.microsoft.com/library/azure/dn857353.aspx).
> 
> 

**Dokumentåtgärder**

* `upload`: En åtgärd för överföringen är liknande tooan ”upsert” där hello dokumentet infogas om det är nytt och uppdatera/ersättas om den finns. Observera att alla fält ersätts i hello update fall.
* `merge`: Sammanslagning uppdateringar ett befintligt dokument med hello angivna fält. Hello merge misslyckas om hello dokumentet inte finns. Alla fält som du anger i en merge ersätter hello befintliga fält i hello. Detta gäller även fält av typen `Collection(Edm.String)`. Till exempel om hello dokumentet innehåller fältet ”taggar” med värdet `["budget"]` och du utföra en koppling med värdet `["economy", "pool"]` ”taggar” hello slutliga värdet för fältet hello ”taggar” blir `["economy", "pool"]`. Kommer det att **inte** vara `["budget", "economy", "pool"]`.
* `mergeOrUpload`: fungerar som `merge` om ett dokument med hello anges nyckeln redan finns i hello index. Om hello dokument inte finns det fungerar som `upload` med ett nytt dokument.
* `delete`: Ta bort tar bort hello angivna dokumentet från hello index. Observera att alla fält som du anger i en `delete` åtgärden än hello nyckelfältet kommer att ignoreras. Om du vill tooremove ett fält från ett dokument, använda `merge` enkelt och i stället ange hello fält explicit för`null`.

**Svar**

Statuskoden 200 returneras (OK) för ett lyckat svar, vilket innebär att alla objekt indexerade. Detta anges av hello `status` egenskapen som anges tootrue för alla objekt, samt hello `statusCode` egenskapen som anges tooeither 201 (för nyligen uppladdade dokument) eller 200 (för sammanfogade eller borttagna dokument):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Statuskoden 207 (flera Status) returnerades när minst ett objekt inte har har indexerats. Objekt som inte har indexerats har hello `status` fältet Ange toofalse. Hej `errorMessage` och `statusCode` egenskaper visar hello indexering fel hello orsak:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "hello search service is too busy tooprocess this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

hello i den följande tabellen beskrivs hello olika per dokument statuskoder som kan returneras hello svar. Observera att vissa anger problem med hello begäran, medan andra indikerar tillfälliga fel. hello senare du kan försöka efter en stund.

<table style="font-size:12">
    <tr>
        <th>Statuskod</th>
        <th>Betydelse</th>
        <th>Återförsökbart</th>
        <th>Anteckningar</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokumentet har har ändrats eller tagits bort.</td>
        <td>Saknas</td>
        <td>Ta bort är <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. Det vill säga även om en Dokumentnyckel inte finns i hello index, leder försöker en borttagningsåtgärd med nyckeln statuskod 200.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokumentet har skapats.</td>
        <td>Saknas</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Ett fel uppstod i hello-dokumentet som hindrade den från att indexeras.</td>
        <td>Nej</td>
        <td>hello felmeddelande hello svar visar fel med hello dokument.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>hello dokument kan inte kopplas eftersom hello angivna nyckeln inte finns i hello index.</td>
        <td>Nej</td>
        <td>Det här felet uppstår inte för överföringar, eftersom de skapar nya dokument och det uppstår inte för borttagningar eftersom de är <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>En versionskonflikt upptäcktes vid försök tooindex ett dokument.</td>
        <td>Ja</td>
        <td>Detta kan inträffa när du försöker tooindex hello samma dokumentet mer än en gång samtidigt.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>hello index är inte tillgänglig för tillfället eftersom den uppdaterades med hello allowIndexDowntime flaggan set too'true'.</td>
        <td>Ja</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Din söktjänst är inte tillgänglig för tillfället, möjligen på grund av tooheavy belastningen.</td>
        <td>Ja</td>
        <td>Koden ska vänta innan du försöker igen i det här fallet eller riskerar du att förlänga hello-tjänsten inte finns.</td>
    </tr>
</table> 

**Obs**: om din klientkod ofta påträffar ett 207 svar, en möjlig orsak är att hello systemet är under belastning. Du kan kontrollera detta genom att kontrollera hello `statusCode` -egenskapen för 503. Om så är fallet hello rekommenderar vi ***begränsning indexering begäranden***. Annars, om indexering trafik inte subside hello system kunde starta avvisar alla begäranden med 503-fel.

Statuskoden 429 anger att du har överskridit kvoten för hello antalet dokument per index. Du måste skapa ett nytt index eller uppgradera för högre kapacitetsgränser.

**Exempel:**

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
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a>Sök igenom dokument
En **Sök** åtgärden utfärdas som en GET eller POST-begäran och anger parametrarna som ger hello kriterier för att välja matchande dokument.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**När toouse bokför i stället för att hämta**

När du använder HTTP GET toocall hello **Sök** API, behöver du toobe medveten om att hello hello URL: en inte kan vara längre än 8 KB. Detta är vanligtvis tillräckligt för de flesta program. Vissa program kan dock ge mycket stora frågor eller OData filteruttryck. För dessa program är med hjälp av HTTP POST ett bättre alternativ eftersom det tillåter större filter och frågor än GET. Med POST, hello antal termer eller satser i en fråga är hello begränsa faktor inte hello storleken för hello rå fråga eftersom hello begäran storleksgräns för POST är ungefär 16 MB.

> [!NOTE]
> Även om storleksgränsen för hello POST-begäran är mycket stora får inte sökningar och filteruttryck vara godtyckligt komplexa. Se [Lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx) och [OData-uttryckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) för mer information om begränsningar för sökning fråge- och komplexitet.
> 
> 

**Förfrågan**

HTTPS krävs för tjänstbegäranden. Hej **Sök** begäran kan konstrueras med hello GET eller POST-metoderna.

URI-begäran hello anger vilka index tooquery, för alla dokument som matchar hello parametrar. Parametrar har angetts på hello frågesträngen hello gäller GET-begäranden och i hello begäran text i hello skiftläget för POST-begäranden.

Ett bra tips när du skapar GET-begäranden, spara för[URL-koda](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifik fråga parametrar när du anropar hello REST API direkt. För **Sök** åtgärder, detta omfattar:

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

URL-kodning rekommenderas endast på hello ovan Frågeparametrar. Om du av misstag URL-koda hello hela frågesträng (allt efter hello?) bryts begäranden.

Dessutom krävs URL-kodning bara när du anropar hello REST API: et direkt med komma. Ingen URL-kodning krävs när du anropar **Sök** med POST, eller när du använder hello [.NET-klientbibliotek](https://msdn.microsoft.com/library/dn951165.aspx), som hanterar URL-kodning för dig.

<a name="SearchQueryParameters"></a>
**Frågeparametrar**

**Sök** accepterar flera parametrar som tillhandahåller frågevillkor och även ange sökbeteendet. Du anger dessa parametrar i hello URL: en frågesträng vid anrop av **Sök** via GET och som JSON-egenskaper i hello begärandetexten vid anrop av **Sök** via POST. hello syntax för vissa parametrar är något annorlunda mellan GET och POST. Skillnaderna beskrivs i tillämpliga fall nedan:

`search=[string]`(valfritt) – hello text toosearch för. Alla `searchable` fält söks som standard om inte `searchFields` har angetts. När du söker `searchable` fält, hello söktext själva tokeniserad så att flera villkor kan avgränsas med blanksteg (till exempel: `search=hello world`). toomatch något villkor använda `*` (det kan vara användbart för booleskt filterfrågor). Om du utesluter den här parametern har samma effekt som du anger för hello`*`. Se [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx) för närmare information om hello söksyntax.

* **Obs**: hello resultat kan ibland vara konstigt när frågor skickas över `searchable` fält. Hej tokenizer innehåller logik toohandle fall vanliga tooEnglish text som apostrofer, kommatecken siffror osv. Till exempel `search=123,456` matchar en enskild term 123,456 i stället för enskilda villkor hello 123 och 456, eftersom kommatecken används som avgränsare tusen för stora tal på engelska. Därför bör du använda blanksteg i stället för skiljetecken tooseparate villkoren i hello `search` parameter.

`searchMode=any|all`(valfritt, standardvärden för`any`) – oavsett om någon eller samtliga hello söktermer måste matchas i ordning toocount hello dokumentet som en matchning.

`searchFields=[string]`(valfritt) – hello lista över kommaavgränsade fältet namn toosearch för hello angiven text. Målfält måste vara markerad som `searchable`.

`queryType=simple|full`(valfritt, standardvärden för`simple`) – när set för ”enkla” söktext tolkas med hjälp av en enkel frågespråk som möjliggör symboler som, + * och ””. Frågor utvärderas över alla sökbara fält (fält som anges i `searchFields`) i varje dokument som standard. När hello frågetypen har angetts för`full` söktext tolkas med hello Lucene frågespråk som tillåter specifika fält och viktat sökningar. Se [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx) och [Lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx) för specifika på hello Sök syntax. 

> [!NOTE]
> Intervallet sökning i hello Lucene frågespråket inte stöds för $filter som ger liknande funktionalitet.
> 
> 

`moreLikeThis=[key]`(valfritt) **Viktigt:** den här funktionen är endast tillgänglig i `2015-02-28-Preview`. Det här alternativet kan inte användas i en fråga som innehåller hello text search parametern `search=[string]`. Hej `moreLikeThis` parametern hittar dokument som är liknande toohello dokument som anges av hello Dokumentnyckel. När en sökning har begärts med `moreLikeThis`, en lista över söktermer genereras baserat på hello frekvens och sällsynt villkoren i hello källdokument. Dessa villkor är och sedan använda toomake hello begäran. Som standard hello innehållet i alla `searchable` fält anses om `searchFields` är används toorestrict vilka fält som genomsöks.  

`$skip=#`(valfritt) – hello antalet Sök resulterar tooskip; Får inte vara större än 100 000. Om du behöver tooscan dokument i följd men kan inte använda `$skip` på grund av toothis begränsning, Överväg att använda `$orderby` på en helt sorterade nyckel och `$filter` med en fråga i stället.

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `skip` i stället för `$skip`.
> 
> 

`$top=#`(valfritt) – hello antalet Sök resulterar tooretrieve. Detta kan användas tillsammans med `$skip` tooimplement klientsidan sidindelning i sökresultaten.

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `top` i stället för `$top`.
> 
> 

`$count=true|false`(valfritt, standardvärden för`false`) – anger om toofetch hello Totalt antal resultat. Detta är hello antal alla dokument som matchar hello `search` och `$filter` parametrar, ignorerar `$top` och `$skip`. Om det här värdet för`true` kan påverka prestanda. Observera att returnerade hello antalet är en uppskattning.

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `count` i stället för `$count`.
> 
> 

`$orderby=[string]`(valfritt) – en lista över uttryck avgränsade med kommatecken toosort hello resultat av. Varje uttryck kan vara ett fältnamn eller ett anrop toohello `geo.distance()` funktion. Varje uttryck kan följas av `asc` tooindicated stigande ordning och `desc` tooindicate fallande. hello standard är stigande ordning. TIES delas av hello matchar resultat av dokument. Om ingen `$orderby` anges hello standardsorteringsordningen är fallande dokumentet matchar poäng. Det finns en gräns på 32-satser för `$orderby`.

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `orderby` i stället för `$orderby`.
> 
> 

`$select=[string]`(valfritt) – en lista över kommaavgränsade fält tooretrieve. Om inget anges ingår alla fält som har markerats som hämtas i hello schemat. Du kan också explicit begära alla fält genom att ange den här parametern för`*`.

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `select` i stället för `$select`.
> 
> 

`facet=[string]`(noll eller fler) - en fältet toofacet av. Alternativt hello strängen kan innehålla parametrar toocustomize hello faceting uttryckt i CSV- `name:value` par. Giltiga parametrar finns:

* `count`(max antal aspekten villkor; standardvärdet är 10). Det finns inget maximum men högre värden medföra en motsvarande prestandaförsämring, särskilt om hello fasetterad fältet innehåller ett stort antal unika villkor.
  * Till exempel: `facet=category,count:5` hämtar hello översta fem kategorier i aspekten resultat.  
  * **Obs**: om hello `count` parameter är mindre än hello antalet unika villkor, hello resultaten kan vara felaktig. Detta är på grund av toohello sätt faceting frågor är fördelade på shards. Öka `count` vanligtvis ökar hello noggrannhet hello termen antal, men på en prestandakostnad.
* `sort`(en av `count` toosort *fallande* efter antal `-count` toosort *stigande* efter antal `value` toosort *stigande* efter värde, eller `-value` toosort *fallande* av värdet)
  * Till exempel: `facet=category,count:3,sort:count` hämtar hello översta tre kategorier i aspekten resultaten i fallande ordning efter hello antalet dokument med varje Ortnamn. Till exempel om hello översta tre kategorier är Budget, översikt och Lyxig, och Budget har 5 träffar översikt har 6 och Lyxig har 4, blir sedan hello buckets i hello ordning översikt, Budget, Lyxig.
  * Till exempel: `facet=rating,sort:-value` producerar buckets för alla möjliga betyg i fallande ordning efter värde. Till exempel om hello klassificeringar är från 1 too5, beställs hello buckets 5, 4, 3, 2, 1, oavsett hur många dokument matchar varje klassificering.
* `values`(pipe-avgränsad numeriskt eller `Edm.DateTimeOffset` värden som anger en dynamisk värdeuppsättningen aspekten post)
  * Till exempel: `facet=baseRate,values:10|20` ger tre buckets: en för grundavgift 0 in toobut exklusive 10, en för 10 in toobut inte inklusive 20 och en för 20 eller högre.
  * Till exempel: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` ger två buckets: en för hotell renoverade före februari 2010 och en för hotell renoverade februari 1 2010 eller senare.
* `interval`(heltal-intervall som är större än 0 för siffror eller `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` för värden för datum-tid)
  * Till exempel: `facet=baseRate,interval:100` producerar buckets baserat på grundavgift intervall i storlek 100. Till exempel om grundpris alla mellan $60 och $600, blir det buckets för 0-100, 200 100, 200 300, 300-400, 400 500 och 500 600.
  * Till exempel: `facet=lastRenovationDate,interval:year` producerar en bucket för varje år när hotell har renoverade.
* `timeoffset`([+-] hh: mm, [+-] hhmm, eller [+-] hh) `timeoffset` är valfritt. Kan endast kombineras med hello `interval` alternativet och endast när tillämpade tooa fält av typen `Edm.DateTimeOffset`. hello värdet anger hello UTC-tid offset tooaccount för inställningar av tid gränser.
  * Till exempel: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` använder hello dag gräns som börjar vid UTC 01:00:00 (midnatt i hello mål tidszon)
* **Obs**: `count` och `sort` kan kombineras i hello samma aspekten specifikation, men de kan inte kombineras med `interval` eller `values`, och `interval` och `values` kan inte kombineras.
* **Obs**: intervall aspekter på datum och tid beräknas baserat på UTC-tid om `timeoffset` har inte angetts. Exempel: för `facet=lastRenovationDate,interval:day`, hello dag gräns som börjar vid 00:00:00 UTC. 

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `facets` i stället för `facet`. Du också ange den som en JSON-matris med strängar som där varje sträng är ett separat aspekten uttryck.
> 
> 

`$filter=[string]`(valfritt) – ett strukturerade sökuttryck standard OData-syntax.

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `filter` i stället för `$filter`.
> 
> 

`highlight=[string]`(valfritt) – en uppsättning csv-fältnamn som används för träffar markeras. Endast `searchable` fält kan användas för träffar markering.

`highlightPreTag=[string]`(valfritt, standardvärden för`<em>`) – en string-tagg som läggs toohit visar. Måste anges med `highlightPostTag`.

> [!NOTE]
> När du anropar **Sök** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).
> 
> 

`highlightPostTag=[string]`(valfritt, standardvärden för`</em>`)-taggen sträng som lägger till toohit visar. Måste anges med `highlightPreTag`.

> [!NOTE]
> När du anropar **Sök** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).
> 
> 

`scoringProfile=[string]`(valfritt) – hello namnet på en bedömningsprofil profil tooevaluate matcha poängen för matchande dokument i ordning toosort hello resultat.

`scoringParameter=[string]`(noll eller fler) – anger hello värden för varje parameter som definierats i funktionen för bedömningsprofil (till exempel `referencePointParameter`) hello formatet `name-value1,value2,...`.

* Till exempel om hello bedömningen profil definierar en funktion med en parameter med namnet ”mylocation” Hej frågesträngsalternativet skulle vara `&scoringParameter=mylocation--122.2,44.8`. hello första dash skiljer hello namn från hello värden i listan när hello andra streck är en del av hello första värde (longitud i det här exemplet).
* För bedömningsprofil parametrar som taggar för förstärkning som kan innehålla kommatecken, kan du escape sådana värden i hello listan med hjälp av enkla citattecken. Om själva hello värdena innehåller enkla citattecken, kan du undanta dem av dubblerade.
  * Till exempel om du har en tagg förstärkning parameter med namnet ”mytag” och du vill tooboost hello-taggen värden ”Hello, O'Brien” och ”Smith” hello frågan alternativ för anslutningssträngen skulle vara `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Observera att citattecken är endast för värden med kommatecken.

> [!NOTE]
> När du anropar **Sök** med efter den här parametern heter `scoringParameters` i stället för `scoringParameter`. Du också ange den som en JSON-matris med strängar som där varje sträng är en separat `name-values` par.
> 
> 

`minimumCoverage`(valfritt, som standard too100) - ett tal mellan 0 och 100 som visar hello procentandelen hello index som måste förses med en sökfråga för hello frågan toobe rapporteras som lyckas. Som standard hello hela index måste vara tillgängliga eller `Search` returneras HTTP-statuskoden 503. Om du ställer in `minimumCoverage` och `Search` lyckas den ska returnera HTTP 200 och innehålla en `@search.coverage` värdet i hello-svar som visar hello procentandelen hello index som ingick i hello-frågan.

> [!NOTE]
> Om parametern tooa värdet ska vara mindre än 100 kan vara användbart för att söka efter tillgänglighet även för tjänster med bara en replik. Dock är inte alla matchande dokument garanterat toobe finns i hello sökresultat. Om sökningen återkallning är viktigare tooyour program än tillgänglighet, så är det bästa tooleave `minimumCoverage` standardinställningen 100.
> 
> 

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

Obs: För den här åtgärden hello `api-version` har angetts som en frågeparameter i hello URL oavsett om du anropar **Sök** med GET eller POST.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour tjänst-URL. Hej **Sök** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

För GET: Ingen.

För POST:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Fortsättning av svar som partiell sökning**

Ibland kan inte Azure Search returnera alla hello begärda resulterar i ett enda Search-svar. Detta kan inträffa av olika skäl, till exempel när hello begäranden för många dokument genom att inte ange `$top` eller ange ett värde för `$top` som är för stor. I sådana fall Azure Search innehåller hello `@odata.nextLink` kommentar i hello svarstexten och även `@search.nextPageParameters` om den har en POST-begäran. Du kan använda hello värdena för dessa anteckningar tooformulate en annan Sök begäran tooget hello nästa del av hello search-svar. Detta kallas en ***fortsättning*** hello ursprungliga sökbegäran och hello kallas vanligtvis anteckningar ***fortsättning token***. Se [hello exemplet nedan](#SearchResponse) mer information om dessa kommentarer och var de förekommer i hello svarstexten hello-syntax. 

hello skäl varför Azure Search kan returnera fortsättning token är implementeringen-specifika och ämne toochange. Robust klienter ska alltid vara klar toohandle fall där färre dokument än väntat returneras en fortsättningstoken är inkluderade toocontinue hämtning av dokument. Även hello Observera att du måste använda samma HTTP-metod som hello ursprungliga begäran i ordning toocontinue. Till exempel om du har skickat en begäran om att hämta alla fortsättning begäranden som du skickar måste också använda GET (och även för POST).

<a name="SearchResponse"></a>
**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

    {
      "@odata.count": # (if $count=true was provided in hello query),
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "@search.facets": { (if faceting was specified in hello query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body toofetch hello next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL toofetch hello next page of results if not all results could be returned in this response; Applies tooboth GET and POST)
    }

**Exempel:**

Du hittar ytterligare exempel på hello [syntaxen för OData-uttryck för Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) sidan.

1)    Sök hello fallande Index sorteras efter datum.

    GET-/indexes/hotels/docs? Sök = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”orderby”: ”lastRenovationDate desc”}

2)    Söka hello index i en fasetterad sökning och hämta facets för kategorier, klassificering, etiketter samt artiklar med baseRate i särskilda intervall:

    GET-/indexes/hotels/docs? Sök = test & aspekten = kategori & aspekten = klassificering & aspekten = taggar & aspekten = baseRate värden: 80 | 150 | 220 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”test”, ”facets”: [”kategori”, ”klassificering”, ”-taggar” ”baseRate värden: 80 | 150 | 220”]}

3)    Med hjälp av ett filter, begränsa hello tidigare fasetterad frågeresultaten när hello användaren klickar på klassificeringen 3 och kategori ”översikt”:

    GET-/indexes/hotels/docs? Sök = test & aspekten = taggar & aspekten = baseRate värden: 80 | 150 | 220 & $filter = klassificering eq 3 och kategori eq 'översikt' & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {”Sök”: ”test”, ”facets”: [”taggar” ”, baseRate värden: 80 | 150 | 220”], ”filter”: ”klassificering eq 3 och kategori eq 'Översikt'”}

4) Ange en övre gräns på unikt villkor som returneras i en fråga i en fasetterad sökning. hello standardvärdet är 10, men du kan öka eller minska det här värdet med hello `count` parameter på hello `facet` attribut:

    GET-/indexes/hotels/docs? Sök = test & aspekten = ort, antal: 5 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”test”, ”facets”: [”Ort, antal: 5”]}

5)    Sök hello Index inom specifika områden. Till exempel ett fält för vissa språk:

    GET-/indexes/hotels/docs? Sök = hôtel & searchFields = description_fr & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”hôtel”, ”searchFields”: ”description_fr”}

6) Sök hello Index över flera fält. Exempelvis kan du lagra och fråga sökbara fält på flera språk och alla inom hello samma index.  Om engelska och franska beskrivningar samexisterar i hello samma dokument, du kan returnera några eller alla i hello frågeresultat:

    GET-/indexes/hotels/docs? Sök = hotell & searchFields = beskrivning, description_fr & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”hotell”, ”searchFields”: ”beskrivning, description_fr”}

Observera att du bara kan fråga ett index i taget. Skapa inte flera index för varje språk om du planerar tooquery en i taget.

7)    Växlingsfil - Get hello 1 sida med objekt (sidstorlek är 10):

    GET-/indexes/hotels/docs? Sök = * & $skip = 0 & $top = 10 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”hoppa över”: 0, ”top”: 10}

8)    Växlingsfil - Get hello 2 sida med objekt (sidstorlek är 10):

    GET-/indexes/hotels/docs? Sök = * & $skip = 10 & $top = 10 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”hoppa över”: ”top” 10: 10}

9)    Hämta en specifik uppsättning fält:

    GET-/indexes/hotels/docs? Sök = * & $select = hotelName, beskrivning och api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: *”, ”Välj”: ”hotelName beskrivning”}

10)  Hämta dokument som matchar ett specifikt filter-uttryck:

    GET-/indexes/hotels/docs? $filter = (baseRate ge 60 och baseRate lt 300) eller hotelName eq 'Avancerad förblir' & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”filter” ”: (baseRate ge 60 och baseRate lt 300) eller hotelName eq 'Avancerad stanna'”}

11) Hello sökindex och returnera fragment med träffar viktiga funktioner

    GET-/indexes/hotels/docs? Sök = något & Markera = beskrivning & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”highlight”: ”beskrivning”}

12) Hello sökindex och returnera dokument sorterade från närmare toofarther från en referensplats

    GET-/indexes/hotels/docs? Sök = något & $orderby=geo.distance (plats, geography'POINT(-122.12315 47.88121)') & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”orderby” ”: geo.distance (plats, geography'POINT(-122.12315 47.88121)')”}

13) Sök hello index under förutsättning att det är en bedömningsprofilen som kallas ”geo” med två avståndsresultatfunktioner, en anger en parameter som kallas ”currentLocation” och en definierar en parameter med namnet ”lastLocation”

    GET-/indexes/hotels/docs? Sök = något & scoringProfile = geo & scoringParameter = currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök”: ”något”, ”scoringProfile”: ”geo”, ”scoringParameters”: [”currentLocation--122.123,44.77233” ”, lastLocation--121.499,44.2113”]}

14) Hitta dokument i hello index med [enkel frågesyntaxen](https://msdn.microsoft.com/library/dn798920.aspx). Den här frågan returnerar hotell där sökbara fält innehåller hello villkoren ”bekvämlighet” och ”plats” men inte ”översikt”:

    GET-/indexes/hotels/docs? Sök = bekvämlighet + plats-översikt & searchMode = all & api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: bekvämlighet + plats-översikt”, ”searchMode”: ”alla”}

Observera hello användning av `searchMode=all` ovan. Inklusive den här parametern åsidosätter hello standardvärdet `searchMode=any`, att se till att som `-motel` innebär ”och inte” i stället för ”eller inte”. Utan `searchMode=all`, får du ”eller inte” som utökar snarare än begränsar sökresultaten och det kan vara krånglig toosome användare.

15) Hitta dokument i hello index med [lucene frågesyntaxen](https://msdn.microsoft.com/library/mt589323.aspx). Den här frågan returnerar hotell där hello kategorifältet innehåller hello termen ”budget” och alla sökbara fält som innehåller hello frasen ”nyligen renoverade”. Dokument som innehåller hello frasen ”nyligen renoverade” rankas högre på grund av hello termen förstärkningen värde (3)

    GET-/indexes/hotels/docs? Sök = kategori: budget och \"nyligen renoverade\"^ 3 & searchMode = all & api-version = 2015-02-28-Preview & querytype = full

    POST /indexes/hotels/docs/search? api-version 2015-02-28-Preview = {”Sök” ”: kategori: budget och \"nyligen renoverade\"^ 3”, ”queryType”: ”full”, ”searchMode”: ”alla”}

<a name="LookupAPI"></a>

## <a name="lookup-document"></a>Sökning dokumentet
Hej **sökning dokumentet** åtgärden hämtar ett dokument från Azure Search. Detta är användbart när en användare klickar på en specifik sökresultat, och du vill toolook in specifik information om dokumentet.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Förfrågan**

HTTPS krävs för tjänstbegäranden. Hej **sökning dokumentet** begäran kan konstrueras enligt följande.

    GET /indexes/[index name]/docs/key?[query parameters]

Du kan också använda hello traditionella OData-syntaxen för nyckelsökningen:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

hello begäran URI innehåller ett [indexnamn] och [], anger vilka dokumentet tooretrieve från vilka index. Du kan bara hämta ett dokument åt gången. Använd **Sök** tooget flera dokument i en enskild begäran.

**Frågeparametrar**

`$select=[string]`(valfritt) – en lista över kommaavgränsade fält tooretrieve. Om angetts eller har angetts för`*`, alla fält som har markerats som hämtas i hello schemat ingår i hello projektion.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

Obs: För den här åtgärden hello `api-version` har angetts som en frågeparameter.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour tjänst-URL. Hej **sökning dokumentet** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

Ingen.

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

    {
      field_name: field_value (fields matching hello default or specified projection)
    }

**Exempel**

Sökning hello dokument har nyckel '2'

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Sökning hello dokument har nyckel ”3” med OData-syntax:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a>Antal dokument
Hej **antalet dokument** åtgärden hämtar antalet hello antalet dokument i en sökindex. Hej `$count` syntax är en del av hello OData-protokollet.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Förfrågan**

HTTPS krävs för tjänstbegäranden. Hej **antalet dokument** begäran kan konstrueras med hello GET-metoden.

Hej [index name] i hello Begärd URI visar hello service tooreturn en uppräkning av alla objekt i hello docs samling hello angivet index.

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden.

* `Accept`: Det här värdet måste anges för`text/plain`.
* `api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour tjänst-URL. Hej **antalet dokument** begäran kan ange en administrationsnyckeln eller Frågenyckeln för `api-key`.

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

Ingen.

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

hello svarstexten innehåller hello count-värdet som ett heltal som är formaterad med oformaterad text.

<a name="Suggestions"></a>

## <a name="suggestions"></a>Förslag
Hej **förslag** åtgärden hämtar förslag baserat på partiell sökning indata. Den används vanligtvis i rutorna tooprovide typ-ahead sökförslag som användare skriver in söktermer.

Förslag begäranden syftet förslag på måldokument som, så hello förslag text upprepas om flera kandidatdokument matchar hello samma söka i indata. Du kan använda `$select` tooretrieve andra dokumentera fält (inklusive hello Dokumentnyckel) så att du kan se vilka dokumentet är hello källa för varje förslag.

En **förslag** utfärdas igen som en GET eller POST-begäran.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**När toouse bokför i stället för att hämta**

När du använder HTTP GET toocall hello **förslag** API, behöver du toobe medveten om att hello hello URL: en inte kan vara längre än 8 KB. Detta är vanligtvis tillräckligt för de flesta program. Men genererar vissa program mycket stora frågor, speciellt OData filteruttryck. För dessa program är med hjälp av HTTP POST ett bättre alternativ eftersom det tillåter filter med större än GET. Med INLÄGGET hello-satser i ett filter är hello begränsande faktor, hello inte storleken på hello rådata Filtersträngen eftersom hello begäran storleksgräns för POST är ungefär 16 MB.

> [!NOTE]
> Även om storleksgränsen för hello POST-begäran är väldigt stor får inte filteruttryck vara godtyckligt komplexa. Se [OData-uttryckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) för mer information om begränsningar för filter komplexitet.
> 
> 

**Förfrågan**

HTTPS krävs för tjänstbegäranden. Hej **förslag** begäran kan konstrueras med hello GET eller POST-metoderna.

URI-begäran hello anger hello index tooquery hello namn. Parametrar, till exempel hello delvis inkommande sökterm anges på hello frågesträngen hello gäller GET-begäranden och i hello begäran text i hello skiftläget för POST-begäranden.

Ett bra tips när du skapar GET-begäranden, spara för[URL-koda](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specifik fråga parametrar när du anropar hello REST API direkt. För **förslag** åtgärder, detta omfattar:

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

URL-kodning rekommenderas endast på hello ovan Frågeparametrar. Om du av misstag URL-koda hello hela frågesträng (allt efter hello?) bryts begäranden.

Dessutom krävs URL-kodning bara när du anropar hello REST API: et direkt med komma. Ingen URL-kodning krävs när du anropar **förslag** med POST, eller när du använder hello [.NET-klientbibliotek](https://msdn.microsoft.com/library/dn951165.aspx), som hanterar URL-kodning för dig.

**Frågeparametrar**

**Förslag** accepterar flera parametrar som tillhandahåller frågevillkor och även ange sökbeteendet. Du anger dessa parametrar i hello URL: en frågesträng vid anrop av **förslag** via GET och som JSON-egenskaper i hello begärandetexten vid anrop av **förslag** via POST. hello syntax för vissa parametrar är något annorlunda mellan GET och POST. Skillnaderna beskrivs i tillämpliga fall nedan:

`search=[string]`-hello text toouse toosuggest sökningar. Måste vara minst 1 tecken och högst 100 tecken.

`highlightPreTag=[string]`(valfritt) – ett sträng-tagg som läggs toosearch träffar. Måste anges med `highlightPostTag`.

> [!NOTE]
> När du anropar **förslag** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).
> 
> 

`highlightPostTag=[string]`(valfritt) – ett sträng-tagg som lägger till toosearch träffar. Måste anges med `highlightPreTag`.

> [!NOTE]
> När du anropar **förslag** använder GET, reserverade tecken i hello URL måste vara procent-kodat (till exempel % 23 i stället för #).
> 
> 

`suggesterName=[string]`-hello namnet på hello förslagsställarens som anges i hello `suggesters` samling som är en del av hello indexdefinitionen. En `suggester` bestämmer vilka fält som genomsöks för föreslagna sökord. Se [Suggesters](#Suggesters) mer information.

`fuzzy=[boolean]`(valfritt, som standard = false)-tootrue när ange detta API hittar förslag även om det finns en saknas eller ersatta tecken i hello söktexten. Detta ger en bättre upplevelse i vissa fall gäller det vid en prestandakostnad eftersom fuzzy förslag sökningar är långsammare och förbruka mer resurser.

`searchFields=[string]`(valfritt) – Ange hello lista med CSV-fältet namn toosearch för hello söktexten. Målfält måste vara aktiverat för förslag.

`$top=#`(valfritt, som standard = 5)-hello antal förslag tooretrieve. Måste vara ett tal mellan 1 och 100.

> [!NOTE]
> När du anropar **förslag** med efter den här parametern heter `top` i stället för `$top`.
> 
> 

`$filter=[string]`(valfritt) - uttryck som filtrerar hello dokument anses vara förslag.

> [!NOTE]
> När du anropar **förslag** med efter den här parametern heter `filter` i stället för `$filter`.
> 
> 

`$orderby=[string]`(valfritt) – en lista över uttryck avgränsade med kommatecken toosort hello resultat av. Varje uttryck kan vara ett fältnamn eller ett anrop toohello `geo.distance()` funktion. Varje uttryck kan följas av `asc` tooindicated stigande ordning och `desc` tooindicate fallande. hello standard är stigande ordning. Det finns en gräns på 32-satser för `$orderby`.

> [!NOTE]
> När du anropar **förslag** med efter den här parametern heter `orderby` i stället för `$orderby`.
> 
> 

`$select=[string]`(valfritt) – en lista över kommaavgränsade fält tooretrieve. Om inget anges endast hello Dokumentnyckel och förslag returneras. Du kan uttryckligen begära alla fält genom att ange den här parametern för`*`.

> [!NOTE]
> När du anropar **förslag** med efter den här parametern heter `select` i stället för `$select`.
> 
> 

`minimumCoverage`(valfritt, som standard too80) - ett tal mellan 0 och 100 som visar hello procentandelen hello index som måste omfattas av en fråga förslag för hello frågan toobe rapporteras som lyckas. Som standard minst 80% av hello index måste vara tillgängliga eller `Suggest` returneras HTTP-statuskoden 503. Om du ställer in `minimumCoverage` och `Suggest` lyckas den ska returnera HTTP 200 och innehålla en `@search.coverage` värdet i hello-svar som visar hello procentandelen hello index som ingick i hello-frågan.

> [!NOTE]
> Om parametern tooa värdet ska vara mindre än 100 kan vara användbart för att söka efter tillgänglighet även för tjänster med bara en replik. Inga matchande förslag är dock garanterat toobe finns i hello resultat. Om återkallning är viktigare tooyour program än tillgänglighet och det är bästa inte toolower `minimumCoverage` nedan standardvärdet 80.
> 
> 

`api-version=[string]`(krävs). hello förhandsversionen är `api-version=2015-02-28-Preview`. Se [Sök Versionhantering](http://msdn.microsoft.com/library/azure/dn864560.aspx) information och alternativa versioner.

Obs: För den här åtgärden hello `api-version` har angetts som en frågeparameter i hello URL oavsett om du anropar **förslag** med GET eller POST.

**Huvuden för begäran**

hello följande lista beskriver hello nödvändiga och valfria begärandehuvuden

* `api-key`: hello `api-key` är används tooauthenticate hello begäran tooyour Search-tjänsten. Det är ett strängvärde, unika tooyour tjänst-URL. Hej **förslag** begäran kan ange en administrationsnyckeln eller Frågenyckeln som hello `api-key`.

Du måste också hello service name tooconstruct hello begärd URL. Du kan hämta hello tjänstnamn och `api-key` från instrumentpanelen service i hello Azure-portalen. Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) sidan navigering hjälp.

**Begärandetexten**

För GET: Ingen.

För POST:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Svar**

Statuskod: 200 OK returneras för ett lyckat svar.

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Om hello projektion alternativet används tooretrieve fält de ingår i varje element i matrisen hello:

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Exempel**

Hämta 5 förslag där hello partiell sökning indata är 'lux'

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
