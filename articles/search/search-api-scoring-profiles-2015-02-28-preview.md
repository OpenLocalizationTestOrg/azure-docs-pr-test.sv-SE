---
title: aaaScoring profiler (Azure Search REST API-Version 2015-02-28-Preview) | Microsoft Docs
description: "Azure Search är en söktjänst för värdbaserade moln som har stöd för inställning av rangordnas resultat baserat på användardefinierade bedömningsprofil profiler."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: bfd62649-13d7-40b3-a8fa-85521a15084d
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.author: heidist
ms.date: 10/27/2016
ms.openlocfilehash: 17f83fdf6818dc6ffcc3e04f5d0185c6f646b63d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Bedömningsprofil profiler (Azure Search REST API-Version 2015-02-28-Preview)
> [!NOTE]
> Den här artikeln beskriver bedömningsprofil profiler i hello [2015-02-28-Preview](search-api-2015-02-28-preview.md). Det finns för närvarande ingen skillnad mellan hello `2016-09-01` version dokumenteras i [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) och hello `2015-02-28-Preview` version beskrivs här, men vi erbjuder det här dokumentet ändå i ordning tooprovide dokumentet täckningen över hello hela API.
>
>

## <a name="overview"></a>Översikt
Bedömningen refererar toohello beräkning av en sökning poäng för varje objekt som returneras i sökresultaten. hello resultatet är en indikator på ett objekt relevans hello gäller hello aktuella sökåtgärden. Hej högre hello poäng, hello mer relevant hello-objektet. Objekt är RANG på hög toolow, baserat på hello Sök poäng beräknas för varje objekt i sökresultaten.

Azure Search använder standard bedömningen toocompute en inledande poäng, men du kan anpassa hello beräkning via en bedömningsprofilen. Bedömningsprofil profiler ger dig större kontroll över hello rangordning objekt i sökresultaten. Du kanske exempelvis vill tooboost objekt baserat på deras potentiella intäkter, befordra nyare element eller kanske öka objekt som har gjorts i lagret är för långt.

En bedömningsprofilen är en del av hello indexdefinitionen består av fält, funktioner och parametrar.

toogive dig en uppfattning om vad en bedömningsprofilen ser ut hello följande exempel visas en profil för enkel namnet 'geo'. Den här förstärker artiklar som har hello sökterm i hello `hotelName` fältet. Använder också hello `distance` fungerar toofavor objekt som ligger inom tio kilometer hello aktuella plats. Om någon söker på hello termen ”inn' och 'inn' händer toobe del av hello hotell namn, visas dokument som innehåller hotell med 'inn' högre upp i hello sökresultat.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

toouse som den här bedömningsprofilen frågan är formulerade toospecify hello profilen på hello frågesträngen. Observera hello frågeparameter i hello frågan nedan, `scoringProfile=geo` i hello-begäran.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Den här frågan söker efter hello termen ”inn' och skickar hello aktuella plats. Observera att den här frågan innehåller andra parametrar som `scoringParameter`. Frågeparametrar beskrivs i [Sök dokument (Azure Search-API)](search-api-2015-02-28-preview.md#SearchDocs).

Klicka på [exempel](#example) tooreview en mer ingående exempel på en bedömningsprofilen.

## <a name="what-is-default-scoring"></a>Vad är standard bedömningen?
Bedömningen beräknar Sök poängen för varje objekt i en rank beställda resultatuppsättning. Alla objekt i sökresultaten är tilldelade en sökning poäng sedan rangordnas högsta toolowest. Objekt med högre hello-resultat returneras toohello program. Som standard hello översta 50 returneras, men du kan använda hello `$top` parametern tooreturn ett större eller mindre antal objekt (upp too1000 i ett enda svar).

Som standard beräknas en sökning poäng utifrån statistiska egenskaper för hello data och hello frågan. Azure Search hittar dokument som innehåller hello sökvillkor i hello frågesträng (vissa eller alla, beroende på `searchMode`), prioriterar dokument som innehåller många instanser av hello sökord. hello Sök poäng går upp även högre om hello termen sällsynta mellan hello data Kristi, men vanliga inom hello dokument. hello basen för den här metoden toocomputing relevans kallas TF IDF eller (termen frekvens inversen dokumentet frekvens).

Under förutsättning att det finns ingen anpassad sortering,-resultat rangordnas sedan Sök poäng innan de returneras toohello anropande programmet. Om `$top` har inte angetts 50 artiklar med hello högsta Sök resultat returneras.

Sök poäng värden kan upprepas i en resultatuppsättning. Du kan till exempel ha 10 artiklar med en poäng för 1.2, 20 artiklar med en poäng 1.0 och 20 artiklar med en poäng för 0,5. När flera träffar har hello samma sökning poäng, hello sorteringen av samma poängsatta objekt har inte definierats och är inte stabilt. Kör hello frågan igen och du kan se objekten flyttas. Två objekt med en identisk poäng är, det ingen garanti vilken visas först.

## <a name="when-toouse-custom-scoring"></a>När toouse anpassade bedömningen
Du bör skapa en eller flera bedömningsprofil profiler när hello standard rangordning beteendet inte gå tillräckligt långt i dina affärsmål. Du kan till exempel bestämma att sökningens relevans ska ge företräde åt nyligen tillagda objekt. På samma sätt kan du ha ett fält som innehåller vinst eller ett annat fält som indikerar potentiella intäkter. Förstärkning träffar som sätta fördelar tooyour företag kan vara en viktig faktor för att bestämma toouse bedömningen profiler.

Relevans-baserade ordning genomförs även via bedömningen profiler. Överväg att sökresultat sidor som du har använt i hello senaste där du kan sortera efter pris, datum, klassificering eller betydelse. I Azure Search enheten bedömningsprofil profiler hello 'relevans-alternativet. hello definition relevanta styrs av du, förutsätter affärsmål och hello typ av sökinställningar du vill toodeliver.

<a name="example"></a>

## <a name="example"></a>Exempel
Som nämndes, implementeras anpassade bedömningen via bedömningen profiler som definierats i ett indexeringsschema.

Det här exemplet visar hello schemat för ett index med två bedömningsprofil profiler (`boostGenre`, `newAndHighlyRated`). Alla frågor mot detta index som innehåller antingen profil eftersom en frågeparameter kommer att använda hello profil tooscore hello resultatmängden.

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Arbetsflöde
tooimplement anpassade bedömningen beteende, lägga till ett bedömningsprofil profil toohello schema som definierar hello index. Du kan ha upp too16 bedömningsprofil profiler i ett index (se [Tjänstbegränsningarna](search-limits-quotas-capacity.md)), men du kan bara ange en profil för närvarande i en given fråga.

Börja med hello [mallen](#bkmk_template) i det här avsnittet.

Ange ett namn. Bedömningsprofil profiler är valfria, men om du lägger till en hello namn måste anges. Vara säker på att toofollow hello namnkonventionerna för fält (startar med en bokstav, undviker specialtecken och reserverade ord). Se [namnregler](http://msdn.microsoft.com/library/azure/dn857353.aspx) för mer information.

hello brödtext hello bedömningen profilen har skapats från viktat fält och funktioner.

### <a name="weights"></a>Vikter
Hej `weights` -egenskapen för en bedömningsprofilen anger namn-värdepar som tilldelar ett relativt viktat tooa fält. I hello [exempel](#example), hello albumTitle och genre artistName fält är ökat 1.5, 5, 2, respektive. Varför genre förstärks så mycket högre än hello andra? Om sökningen genomförs över data som är något homogen (vilket är fallet hello med genre om du i hello `musicstoreindex`), måste du kanske en större variationen i hello relativa vikten. Till exempel i hello `musicstoreindex`, 'Berg ”visas som båda genre och i identiskt fraserats genre beskrivningar. Om du vill ha genre toooutweigh genre beskrivning måste hello genre fältet en mycket högre relativa viktade.

### <a name="functions"></a>Funktioner
Funktioner som används när det krävs ytterligare beräkningar för specifika sammanhang. Giltig funktionstyper är `freshness`, `magnitude`, `distance` och `tag`. Varje funktion har parametrar som är unika tooit.

* `freshness`ska användas när du vill använda tooboost av hur nya eller gamla ett objekt är. Den här funktionen kan bara användas med datetime-fält (`Edm.DataTimeOffset`). Obs hello `boostingDuration` attributet används endast för hello dokumentens funktion.
* `magnitude`ska användas när du vill använda tooboost baserat på hur hög eller låg ett numeriskt värde är. Scenarier som anropar för den här funktionen är förstärkning av vinst, högsta pris, lägsta pris eller antalet hämtningar. Om du vill att hello inverterade mönster (till exempel tooboost lägre priser objekt mer än högre prisvärda objekt) kan du återställa hello intervallet, hög toolow. Den angivna olika priser mellan 100 för$ 1, ska ställas in `boostingRangeStart` 100 och `boostingRangeEnd` på 1 tooboost hello lägre priser objekt. Den här funktionen kan endast användas med dubbla och heltal.
* `distance`ska användas när du vill tooboost av närhet eller geografisk plats. Den här funktionen kan bara användas med `Edm.GeographyPoint` fält.
* `tag`ska användas när du vill tooboost efter taggar mellan dokument och sökningar. Den här funktionen kan bara användas med `Edm.String` och `Collection(Edm.String)` fält.

#### <a name="rules-for-using-functions"></a>Regler för med hjälp av funktioner
* Funktionstyp (dokumentens, omfattning, avstånd, taggen) måste vara versaler.
* Funktioner kan inte innehålla null eller tomma värden. I synnerhet om du inkluderar fältnamn har tooset den toosomething.
* Funktioner kan bara vara tillämpade toofilterable. Se [Create Index](search-api-2015-02-28-preview.md#CreateIndex) för mer information om filtrera fält.
* Funktioner kan bara vara tillämpade toofields som definieras i hello fältsamlingen för ett index.

När hello index har definierats, skapa hello index genom att överföra hello indexeringsschema följt av dokument. Se [Create Index](search-api-2015-02-28-preview.md#CreateIndex) och [Lägg till eller uppdatera dokument](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) anvisningar för dessa åtgärder. När du har skapat indexet hello, bör du ha en fungerande bedömningsprofilen som fungerar med din sökning-data.

<a name="bkmk_template"></a>

## <a name="template"></a>Mall
Detta avsnitt visar hello syntax och mallen för resultatfunktioner profiler. Se för[Index attributreferensen](#bkmk_indexref) i nästa avsnitt om hello beskrivningar av hello-attribut.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies toosearchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location)
              "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>

## <a name="scoring-profile-property-reference"></a>Referens för bedömningsprofil profil
> [!NOTE]
> En bedömningsprofil funktion kan endast vara tillämpade toofields som är filtrera.
>
>

| Egenskap | Beskrivning |
| --- | --- |
| `name` |Krävs. Det här är hello hello bedömningen profil. Den här hello samma mönster för ett fält. Den måste börja med en bokstav, får inte innehålla punkter, kolon eller @ symboler, och kan inte börja med hello frasen ”azureSearch” (skiftlägeskänsligt). |
| `text` |Innehåller hello vikterna egenskap. |
| `weights` |Valfri. Ett namn-värde-par som anger ett fältnamn och relativa viktade. Relativa viktade måste vara ett positivt heltal eller flyttal. Du kan ange hello fältnamn utan en motsvarande vikt. Vikterna är används tooindicate hello vikten av relativa tooanother för ett fält. |
| `functions` |Valfri. Observera att en bedömningsprofil funktion kan endast tillämpas toofields som är filtrera. |
| `type` |Krävs för resultatfunktioner funktioner. Anger hello typ av funktionen toouse. Giltiga värden är `magnitude`, `freshness`, `distance` och `tag`. Du kan ta mer än en funktion i varje bedömningsprofilen. hello funktionsnamn måste vara versaler. |
| `boost` |Krävs för resultatfunktioner funktioner. Ett positivt tal används som multiplikator för råvärde. Det får inte vara lika too1. |
| `fieldName` |Krävs för resultatfunktioner funktioner. En bedömningsprofil funktion kan endast vara tillämpade toofields som ingår i hello fältet mängd hello index och som är filtrera. Dessutom kan introducerar varje funktionstyp ytterligare begränsningar (dokumentens används med datetime-fält omfattning med heltal eller dubbel fält, avstånd med plats och tagga med sträng eller samling strängfält). Du kan bara ange ett enda fält per funktionsdefinitionen. För exempelvis toouse omfattning två gånger i Hej samma profil, behöver du tooinclude två definitioner omfattning, ett för varje fält. |
| `interpolation` |Krävs för resultatfunktioner funktioner. Definierar hello lutning för vilken hello resultatförstärkningen mellan ökar, från hello intervallet toohello intervallslut hello hello start. Giltiga värden är `linear` (standard), `constant`, `quadratic`, och `logarithmic`. Se [ange interpolations](#bkmk_interpolation) mer information. |
| `magnitude` |hello är bedömningen funktion används tooalter rangordningar baserat på hello värdeintervallet för ett numeriskt fält. Några av hello vanligaste användningsexempel på detta är:<ul><li>Om du hämtar stjärnklassificeringar: Alter hello bedömningen baserat på hello värde inom hello ”Star klassificeringen” fält. När två objekt är relevanta visas hello-objekt med högre klassificering av hello först.</li><li>Marginal: När två dokument är relevant, en återförsäljare kanske vill tooboost dokument som har högre marginaler först.</li><li>Klicka på antalet: program som spårar Klicka på via åtgärder tooproducts eller sidor, kan du använda omfattning tooboost objekt som brukar tooget hello merparten av trafiken.</li><li>Hämta antal: för program som spårar nedladdningar, hello omfattning funktionen kan du öka objekt som har hello de flesta hämtningar.</li></ul> |
| `magnitude:boostingRangeStart` |Anger hello starta värdet för hello intervallet över vilket omfattning beräknas. hello-värdet måste vara ett heltal eller ett flyttal. För stjärnklassificering mellan 1 och 4 skulle värdet vara 1. För en marginal över 50% skulle värdet vara 50. |
| `magnitude:boostingRangeEnd` |Anger hello slutvärdet för hello intervallet över vilket omfattning beräknas. hello-värdet måste vara ett heltal eller ett flyttal. För stjärnklassificering mellan 1 och 4 skulle värdet vara 4. |
| `magnitude:constantBoostBeyondRange` |Giltiga värden är SANT eller FALSKT (standard). Om värdet är tootrue, hello hela förstärkningen tooapply toodocuments som har ett värde för hello målfältet som är högre än hello övre intervallslut hello. Om värdet är false hello förstärkningen på inte tillämpade toodocuments med ett värde för hello målfältet som ligger utanför intervallet för hello. |
| `freshness` |hello aktualitet bedömningen funktionen ligger används tooalter rangordning poängen för objekt baserat på värden i DateTimeOffset-fälten. Till exempel kan ett objekt med ett senare datum tilldelas en högre än äldre objekt. (Observera att det är också möjligt toorank objekt som kalenderhändelser med framtida datum så att objekt närmare toohello finns kan tilldelas högre än objekt ytterligare en hello framtida.) Hello aktuella versionen av tjänsten korrigeras en intervallslut hello toohello aktuell tid. hello andra änden är en tid i hello senaste baserat på hello `boostingDuration`. tooboost flera gånger i hello framtida använder en negativ `boostingDuration`. hello frekvens vid vilken hello förstärkning ändringar från en högsta och lägsta intervallet bestäms av hello interpolerade tillämpas toohello bedömningen profil (se hello figuren nedan). tooreverse hello den faktor som tillämpas, Välj en förstärkningen faktor på mindre än 1. |
| `freshness:boostingDuration` |Anger en utgångsperiod efter vilken förstärkning tar slut för ett visst dokument. Se [ange boostingDuration](#bkmk_boostdur) i hello efter avsnittet syntax och exempel. |
| `distance` |hello avstånd bedömningen funktion är används tooaffect hello poängsättningen för dokument baserat på hur stänga eller långt ifrån de är relativt tooa geografisk referensplats. Hej referensplats anges som en del av hello fråga i en parameter (med hjälp av hello `scoringParameter` Frågeparametern) som en celligt lat argumentet. |
| `distance:referencePointParameter` |En parameter toobe angavs i frågor toouse som referensplats. scoringParameter är en frågeparameter. Se [Sök dokument](search-api-2015-02-28-preview.md#SearchDocs) beskrivningar av Frågeparametrar. |
| `distance:boostingDistance` |Ett tal som anger hello avståndet, i kilometer, från hello referens där hello förstärkning intervallet slutar. |
| `tag` |hello bedömningen funktionen används tooaffect hello poängsättningen för dokument baserat på taggar i dokument och sökningar. Att kommer ökat dokument som innehåller taggar gemensamt med hello sökfråga. Hej taggar för hello sökfråga tillhandahålls som en bedömningsprofil parameter i varje sökbegäran (med hjälp av hello `scoringParameter` Frågeparametern). |
| `tag:tagsParameter` |En parameter toobe som används i frågor för toospecify taggar för en viss begäran. `scoringParameter`är en frågeparameter. Se [Sök dokument](search-api-2015-02-28-preview.md#SearchDocs) beskrivningar av Frågeparametrar. |
| `functionAggregation` |Valfri. Gäller endast när funktioner har angetts. Giltiga värden är: `sum` (standard), `average`, `minimum`, `maximum`, och `firstMatching`. En sökning poäng är ett värde som beräknats från flera variabler, inklusive flera funktioner. Detta attribut anger hur hello ökar om funktioner som hello kombineras till en enda sammanställd förstärkningen är tillämpade toohello basera dokumentet poäng. hello-resultat baseras på hello tf idf-värde som beräknats från hello dokumentet och hello sökfråga. |
| `defaultScoringProfile` |När du kör en sökbegäran om inga bedömningsprofilen anges, är standard bedömningen används (tf-idf endast). Standard bedömningen profilnamn kan anges här, medför Azure Search toouse den här profilen om ingen specifik profil anges i hello sökbegäran. |

<a name="bkmk_interpolation"></a>

## <a name="set-interpolations"></a>Ange interpolations
Interpolations Tillåt toodefine hello lutning för vilken hello resultatförstärkningen mellan ökar, från hello intervallet toohello intervallslut hello hello start. Du kan använda följande interpolations hello:

* `Linear`: Hello förstärkningen tillämpas toohello objektet kommer att göras inom en ständigt minskande för objekt som ligger inom Hej max och min intervall. Linjär är hello standard interpolerade för en bedömningsprofilen.
* `Constant`: För artiklar som ligger inom hello start- och avslutar intervall, blir en konstant förstärkning tillämpade toohello rank resultat.
* `Quadratic`: I jämförelse tooa linjär interpolerade som har en ständigt minskande förstärkningen kommer först att minska andragradsekvation i mindre takt och sedan eftersom den närmar sig hello intervallslut minskas med ett mycket högre intervall. Det här alternativet om interpolerade tillåts inte i taggen bedömningen funktioner.
* `Logarithmic`: I jämförelse tooa linjär interpolerade som har en ständigt minskande förstärkningen logaritmisk minskar ursprungligen i högre takt och sedan eftersom den närmar sig hello intervallslut minskas med ett mycket mindre intervall. Det här alternativet om interpolerade tillåts inte i taggen bedömningen funktioner.

<a name="Figure1"></a> ![][1]

<a name="bkmk_boostdur"></a>

## <a name="set-boostingduration"></a>Ange boostingDuration
`boostingDuration`är ett attribut för hello dokumentens funktionen. Du använder den tooset en utgångsperiod efter vilken förstärkning tar slut för ett visst dokument. Tooboost en produktserie eller varumärken under 10 dagar erbjudanden, skulle du till exempel ange hello 10 dagar som ”P10D” för dessa dokument. Eller ange tooboost kommande händelser i hello nästa vecka ”-P7D”.

`boostingDuration`måste formateras som ett XSD ”daytimeduration” XSD-värde (en begränsad delmängd av ett ISO 8601-varaktighetsvärde). hello mönstret för detta är: `[-]P[nD][T[nH][nM][nS]]`.

hello följande tabell innehåller några exempel.

| Varaktighet | boostingDuration |
| --- | --- |
| 1 dag |”P1D” |
| 2 dagar och 12 timmar |”P2DT12H” |
| 15 minuter |”PT15M” |
| 30 dagar, 5 timmar, 10 minuter och 6.334 sekunder |”P30DT5H10M6.334S” |

Fler exempel finns [XML-schemat: datatyper (W3.org webbplats)](http://www.w3.org/TR/xmlschema11-2/).

**Se även**
[Azure Söktjänsts-REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) på MSDN <br/>
[Skapa Index (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) på MSDN<br/>
[Lägg till en bedömningsprofil profil tooa sökindex](http://msdn.microsoft.com/library/azure/dn798928.aspx) på MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
