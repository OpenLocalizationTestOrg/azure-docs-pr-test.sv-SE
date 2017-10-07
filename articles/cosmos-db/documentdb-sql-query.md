---
title: "aaaSQL frågor om Azure Cosmos DB DocumentDB API | Microsoft Docs"
description: "Lär dig mer om SQL-syntax, databasbegrepp och SQL-frågor för Azure Cosmos DB. SQL kan användas som en JSON-frågespråket i Azure Cosmos-databasen."
keywords: "SQL-syntax, sql-fråga, sql-frågor, json-frågespråket, databasbegrepp och sql-frågor, mängdfunktioner"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: arramac
ms.openlocfilehash: f4db95b87f5796c4e4299aaf016435cb6301bbfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a>SQL-frågor för Azure Cosmos DB DocumentDB API
Microsoft Azure Cosmos DB stöder förfrågningar till dokument med SQL (Structured Query Language) som ett JSON-frågespråk. Cosmos DB är verkligen schemafria. Tack vare dess åtagande toohello JSON-datamodell direkt i databasmotorn hello ger automatisk indexering av JSON-dokument utan explicita schema eller att sekundärindex. 

När du utformar hello frågespråk för Cosmos DB hade vi två mål i åtanke:

* I stället för inventing en ny JSON-frågespråket ville vi toosupport SQL. SQL är ett av hello bekant och mest populära språk för frågan. Cosmos-Databasens SQL är en formell programmeringsmodell för komplexa frågor via JSON-dokument.
* Som en JSON-dokument databas kan utföra JavaScript direkt i databasmotorn hello ville vi toouse JavaScript programmeringsmodell som hello foundation våra query Language. Hej DocumentDB API SQL är rotad i typsystemet i Javascript's, utvärdering av uttryck och funktionsanrop. Detta i sin tur är en fysisk programmeringsmodell för relationella projektioner, hierarkisk navigering i JSON-dokument, self kopplingar, spatial frågor och anrop av användardefinierade funktioner (UDF) helt skrivna i JavaScript, bland andra funktioner. 

Vi tror att dessa funktioner är viktiga tooreducing hello friktion mellan hello programmet och hello databasen och är avgörande för utvecklarproduktivitet.

Vi rekommenderar att komma igång genom att titta på hello följande video, där Aravind Ramachandran visar Cosmos DB frågar funktioner, och genom att besöka vårt [Query Playground](http://www.documentdb.com/sql/demo), där du kan prova att använda Cosmos-DB och köra SQL-frågor mot vår datauppsättning.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

Gå sedan tillbaka toothis artikel, där vi börjar med en SQL-fråga självstudiekurs som vägleder dig igenom några enkla JSON-dokument och SQL-kommandon.

## <a id="GettingStarted"></a>Komma igång med SQL-kommandon i Cosmos DB
toosee Cosmos-Databasens SQL på fungerar, vi börjar med några enkla JSON-dokument och gå igenom några enkla frågor mot den. Överväg att dessa två JSON-dokument om två serier. Med Cosmos DB behöver vi inte toocreate alla scheman eller sekundärindex explicit. Vi behöver tooinsert hello JSON-dokument tooa Cosmos DB samling och sedan fråga. Här är ett enkelt JSON-dokumentet för hello familjen Andersen, hello överordnade, underordnade (och deras husdjur) adress och registreringsinformation. hello dokument har strängar, siffror, booleska värden, matriser och kapslade egenskaper. 

**Dokumentet**  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Här är ett andra dokument med en skillnaden mellan – `givenName` och `familyName` används i stället för `firstName` och `lastName`.

**Dokumentet**  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

Nu ska vi prova några frågor mot den här data toounderstand vissa hello nyckeln aspekter av DocumentDB API SQL. Till exempel hello följande fråga returnerar hello dokument där hello id-fältet matchar `AndersenFamily`. Eftersom det är en `SELECT *`, hello utdata från frågan hello är hello fullständig JSON-dokumentet:

**Fråga**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Tänk dig nu hello fallet när vi behöver tooreformat hello JSON-utdata i en annan form. Den här frågan projekt nya JSON-objekt med två valda fält, namn och ort, när hello adress ort har hello samma namn som hello tillstånd. I det här fallet matchar ”NY, NY”.

**Fråga**    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Resultat**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


hello nästa fråga returnerar alla hello angivet namn på barn i hello familj vars id matchar `WakefieldFamily` sorterade efter hello ort där du bor.

**Fråga**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Resultat**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Vi vill gärna toodraw uppmärksamhet tooa några anmärkningsvärda aspekter av hello Cosmos-DB-frågespråket genom hello-exempel som vi har sett:  

* Eftersom DocumentDB API SQL fungerar på JSON-värden, behandlar trädet Formats entiteter i stället för rader och kolumner. Därför hello språket kan du se toonodes hello träd på varje godtycklig djup som `Node1.Node2.Node3…..Nodem`, liknande toorelational SQL refererande toohello två del referens för `<table>.<column>`.   
* hello strukturerade query language fungerar med schemat mindre data. Därför hello typen system måste toobe gränsen dynamiskt. hello kan samma uttryck ge olika typer på olika dokument. hello resultatet av en fråga är ett giltigt JSON-värde, men är inte säkert toobe för ett fast schema.  
* Cosmos DB stöder endast strikt JSON-dokument. Det innebär hello typsystemet och uttryck är begränsad toodeal endast med JSON-typer. Se toohello [JSON-specifikationen](http://www.json.org/) för mer information.  
* En Cosmos-DB-samling är en schemafria behållare för JSON-dokument. hello relationer i data enheter inom och mellan dokument i en samling fångas implicit av inneslutning och inte av primärnyckel och sekundärnyckel viktiga relationer. Det här är en viktig aspekt värt pekar hänsyn hello intra-dokumentet kopplingar som beskrivs senare i den här artikeln.

## <a id="Indexing"></a>Cosmos DB indexering
Innan vi går in hello DocumentDB API SQL-syntax som det är värt att utforska hello indexering design i Cosmos-databasen. 

hello syftar-databasindex tooserve frågorna i de olika formulär och former med minsta resursförbrukning (t.ex. CPU och in-/ utdata) och ger bra genomflöde och låg latens. Hello valet av hello rätt index för att fråga en databas kräver ofta mycket planering och experiment. Den här metoden innebär en utmaning för schemat mindre databaser där hello data stämmer inte överens tooa strikt schema och utvecklas snabbt samtidigt. 

När vi hello Cosmos DB indexering undersystemet, ange vi därför hello följande mål:

* Indexera dokument utan schemat: hello indexering undersystemet inte kräver någon schemainformation eller göra några antaganden om schemat hello dokument. 
* Stöd för effektiv och omfattande hierarkiska och relationella: hello index stöder hello Cosmos-DB-frågespråket effektivt, inklusive stöd för hierarkiska och relationella projektioner.
* Stöd för konsekvent frågor in face of en varaktig volym av skrivningar: för hög skrivåtgärder genomströmning arbetsbelastningar med konsekvent frågor, hello indexet uppdateras inkrementellt effektivt och online i hello yta med en varaktig volym på skrivningar. hello konsekvent indexuppdatering är avgörande tooserve hello frågor på hello konsekvenskontroll nivå i vilken hello konfigurerade användaren hello dokumenttjänst.
* Stöd för flera innehavare: hello reservation-baserade modellen för resursen styrning angivna mellan klienter, utförs index uppdateringar hello budget systemresurser (processor, minne och i/o-åtgärder per sekund) fördelas per replik. 
* Lagringseffektivitet: för kostnadseffektivitet, hello på disken lagring omkostnader av hello index är begränsad och förutsägbara. Det är viktigt eftersom Cosmos DB tillåter hello developer toomake kostnad-baserade kompromisser mellan index kostnader i relationen toohello frågeprestanda.  

Se toohello [Azure Cosmos DB prover](https://github.com/Azure/azure-documentdb-net) på MSDN efter exempel som visar hur tooconfigure hello indexprincip för en samling. Nu är dags hello detaljer om hello Azure Cosmos-Databasens SQL-syntax.

## <a id="Basics"></a>Grunderna i Azure Cosmos-Databasens SQL-fråga
Varje fråga består av en SELECT-satsen och valfria FROM och WHERE-satser per ANSI SQL-standarder. Vanligtvis för varje fråga räknas hello källa i FROM-satsen hello upp. Sedan hello filtrera i WHERE-satsen har tillämpats på hello källa tooretrieve hello en delmängd av JSON-dokument. Slutligen hello SELECT-satsen används tooproject hello begärt JSON-värden i hello select-listan.

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a id="FromClause"></a>FROM-satsen
Hej `FROM <from_specification>` satsen är valfri såvida hello källa filtreras eller planerat senare i hello-frågan. hello syftet med den här satsen är toospecify hello datakälla på vilka hello frågan måste fungera. Hello hela samlingen är ofta hello källan, men ett kan du ange en del av hello samling i stället. 

En fråga som `SELECT * FROM Families` anger att hello hela familjer samlingen är hello källa över vilka tooenumerate. En särskild identifierare rot kan vara används toorepresent hello samlingen istället för att använda hello samlingens namn. hello innehåller följande lista hello regler som tillämpas per fråga:

* hello-samling kan ha ett alias, t.ex `SELECT f.id FROM Families AS f` eller helt enkelt `SELECT f.id FROM Families f`. Här `f` är hello motsvarande `Families`. `AS`är en valfri nyckelordet tooalias hello identifierare.
* En gång alias hello ursprungliga källan kan inte bindas. Till exempel `SELECT Families.id FROM Families f` är syntaktiskt felaktig eftersom hello identifierare ”familjer” inte kan matchas längre.
* Alla egenskaper som behöver toobe refererar till måste vara fullständigt kvalificerad. Hello saknas av strikt schema, detta är tvingande tooavoid några tvetydig bindningar. Därför `SELECT id FROM Families f` är syntaktiskt felaktig sedan hello egenskapen `id` är inte bunden.

### <a name="subdocuments"></a>Underdokument
hello kan också vara minskade tooa mindre deluppsättning. Till exempel tooenumerating endast en underkatalog i varje dokument hello subroot sedan bli hello källa, som visas i följande exempel hello:

**Fråga**

    SELECT * 
    FROM Families.children

**Resultat**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Medan hello ovan exempel används en matris som hello källa, ett objekt även ska användas som hello källa, vilket är vad som visas i följande exempel hello: någon giltig JSON-värde (inte Odefinierad) som finns i hello källan betraktas som ska ingå i hello resultatet av hello-fråga. Om vissa familjer inte har en `address.state` värde, de ingår inte i hello frågeresultat.

**Fråga**

    SELECT * 
    FROM Families.address.state

**Resultat**

    [
      "WA", 
      "NY"
    ]


## <a id="WhereClause"></a>WHERE-satsen
Hej WHERE-satsen (**`WHERE <filter_condition>`**) är valfritt. Det anger hello villkor att hello JSON-dokument som tillhandahålls av hello källa måste uppfylla i ordning toobe ingår som en del av hello resultat. JSON-dokument måste utvärderas hello anges villkor för ”true” toobe för hello resultat. Hej där satsen används av hello index lager i ordning toodetermine hello absolut minsta delmängd källdokument som kan vara en del av hello resultatet. 

hello följande begäranden dokument som innehåller en namnegenskapen vars värde är `AndersenFamily`. Alla dokument som inte har en namnegenskap, eller där hello värdet matchar inte `AndersenFamily` utesluts. 

**Fråga**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


hello föregående exempel visade en enkel likheten fråga. DocumentDB API SQL stöder också en mängd skalära uttryck. de vanligaste hello är binär och unära uttryck. Egenskapsreferenser från hello källa JSON-objekt är också giltigt uttryck. 

följande binära operatorer hello stöds för närvarande och kan användas i frågor som visas i följande exempel hello:  

<table>
<tr>
<td>Aritmetiska</td>    
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Binärt</td>    
<td>|, &, ^, <<>>,, >>> (noll fill högerskift)</td>
</tr>
<tr>
<td>Logiska</td>
<td>OCH, ELLER INTE</td>
</tr>
<tr>
<td>Jämförelse</td>    
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Sträng</td>    
<td>|| (sammanfoga)</td>
</tr>
</table>  


Låt oss ta en titt på några frågor med binära operatorer.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Hej unära operatorer +,-, ~ inte stöds också och kan användas inuti frågor som visas i följande exempel hello:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Dessutom toobinary och unära operatorer, egenskapsreferenser också tillåts. Till exempel `SELECT * FROM Families f WHERE f.isRegistered` returnerar hello JSON-dokument som innehåller hello egenskapen `isRegistered` där hello egenskapens värde är lika toohello JSON `true` värde. Andra värden (FALSKT, null, Odefinierad, `<number>`, `<string>`, `<object>`, `<array>`osv) leder toohello källdokument som ska uteslutas från hello resultat. 

### <a name="equality-and-comparison-operators"></a>Jämförelse av och likhetsfrågor operatörer
hello följande tabell visar hello resultatet av likheten jämförelser i DocumentDB API SQL mellan två typer som JSON.

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Odefinierad</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Booleskt värde</strong>
         </td>
         <td valign="top">
            <strong>Antal</strong>
         </td>
         <td valign="top">
            <strong>Sträng</strong>
         </td>
         <td valign="top">
            <strong>Objektet</strong>
         </td>
         <td valign="top">
            <strong>Matris</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Odefinierad<strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null<strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
            <strong>OKEJ</strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Booleskt värde<strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
            <strong>OKEJ</strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Antal<strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
            <strong>OKEJ</strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Sträng<strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
            <strong>OKEJ</strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objektet<strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
            <strong>OKEJ</strong>
         </td>
         <td valign="top">
Odefinierad </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Matris<strong>
         </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
Odefinierad </td>
         <td valign="top">
            <strong>OKEJ</strong>
         </td>
      </tr>
   </tbody>
</table>

För andra jämförelseoperatorer som >, > =,! =, < och < =, hello gäller följande regler:   

* Jämförelse mellan typer resulterar i Undefined.
* Jämförelse mellan två objekt eller två matriser resulterar i Undefined.   

Om hello resultatet av hello skalärt uttryck i hello filter är odefinierad, hello motsvarande dokumentet inte tas med i hello resultat eftersom Undefined inte logiskt del för ”true”.

### <a name="between-keyword"></a>MELLAN nyckelord
Du kan också använda hello mellan nyckelordet tooexpress frågor mot intervall med värden som i ANSI SQL. MELLAN kan användas mot strängar eller siffror.

Den här frågan returnerar till exempel alla family dokument i vilka hello första underordnade klass är mellan 1-5 (båda inkluderande). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Till skillnad från i ANSI-SQL, kan du också använda hello BETWEEN-satsen i hello-FROM-sats som hello följande exempel.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Kom ihåg toocreate en indexprincip som använder en intervallet Indextypen mot alla numeriska egenskaper/sökvägar som är filtrerade i hello BETWEEN-satsen för snabbare frågan körningstider. 

hello största skillnaden mellan att använda BETWEEN i DocumentDB-API och ANSI SQL är att du kan ange intervallet frågor mot egenskaper för olika typer – du kan till exempel ha ”klass” vara ett tal (5) i vissa dokument och strängar i andra (”grade4”). I dessa fall, som i JavaScript, en jämförelse mellan två olika typer resultaten i ”Odefinierad” och hello dokumentet kommer att hoppas över.

### <a name="logical-and-or-and-not-operators"></a>Logisk (AND, OR och inte) operatorer
Logiska operatorer fungerar med booleska värden. hello logiska sanningen tabeller för de här operatorerna visas i följande tabeller hello.

| ELLER | True | False | Odefinierad |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Odefinierad |
| Odefinierad |True |Odefinierad |Odefinierad |

| OCH | True | False | Odefinierad |
| --- | --- | --- | --- |
| True |True |False |Odefinierad |
| False |False |False |False |
| Odefinierad |Odefinierad |False |Odefinierad |

| INTE |  |
| --- | --- |
| True |False |
| False |True |
| Odefinierad |Odefinierad |

### <a name="in-keyword"></a>I nyckelord
hello IN-nyckelordet kan vara används toocheck om ett angivet värde matchar något värde i en lista. Den här frågan returnerar till exempel alla family dokument där hello-id är ”WakefieldFamily” eller ”AndersenFamily”. 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Det här exemplet returnerar alla dokument där hello tillstånd är någon hello angivna värden.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternär (?) och ansvariga för Coalesce (?)
hello Ternär och Coalesce operatörer kan vara används toobuild villkorsuttryck, liknande toopopular programmeringsspråk som C# och JavaScript. 

hello Ternär (?) operator kan vara väldigt användbar när konstruera nya JSON-egenskaper på hello föra. Till exempel kan nu du skriva frågor tooclassify hello klassen nivåer i en mänsklig läsbar form som nybörjare/mellanliggande/Avancerat enligt nedan.

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Du kan också kapsla hello anrop toohello operator som i hello frågan nedan.

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Som med andra frågeoperatorer ingår om hello refererade egenskaper i hello villkorsuttrycket saknas i alla dokument, eller om hello typer som jämförs är olika, sedan dessa dokument inte i hello frågeresultat.

Hej Coalesce (?)-operatorn kan vara används tooefficiently Kontrollera (kallas även för hello förekomsten av en egenskap har definierats) i ett dokument. Detta är användbart när frågor körs mot halvstrukturerade data av olika typer eller. Den här frågan returnerar till exempel hello ”efternamn” om det finns, eller hello ”efternamn” om det inte finns.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a id="EscapingReservedKeywords"></a>Angiven egenskapsaccessor
Du kan också komma åt egenskaper med hjälp av hello inom citattecken egenskapen operatorn `[]`. Till exempel `SELECT c.grade` och `SELECT c["grade"]` är likvärdiga. Den här syntaxen är användbart när du behöver tooescape en egenskap som innehåller blanksteg, specialtecken eller händer tooshare hello samma namn som en SQL-nyckelord eller reserverat ord.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a id="SelectClause"></a>SELECT-satsen
hello SELECT-satsen (**`SELECT <select_list>`**) är obligatoriskt och anger vilka värden hämtas från hello fråga, precis som i ANSI SQL. hello delmängd som har filtrerats ovanpå hello källdokument har skickats till hello projektion fas där hello angivna JSON-värden hämtas och ett nytt JSON-objekt skapas för varje inmatningen som skickades till den. 

hello som följande exempel visar en typisk urvalsfråga. 

**Fråga**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Kapslade egenskaper
I följande exempel hello, vi projicerar två kapslade egenskaper `f.address.state` och `f.address.city`.

**Fråga**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projektion stöder också JSON-uttryck som visas i följande exempel hello:

**Fråga**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Nu ska vi titta på hello roll `$1` här. Hej `SELECT` sats måste toocreate ett JSON-objekt och eftersom ingen nyckel tillhandahålls vi använder implicit argumentet variabelnamn som börjar med `$1`. Den här frågan returnerar till exempel två implicit argumentvariabler, etikett `$1` och `$2`.

**Fråga**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Alias
Nu ska vi utöka hello exemplet ovan med explicit alias med värden. EFTERSOM är hello nyckelord som används för alias. Det är valfritt som visas när projicera hello andra värdet som `NameInfo`. 

Om en fråga har två egenskaper med hello samma namn, alias måste vara används toorename något eller båda av hello egenskaper så att de skiljas åt i hello planerat resultat.

**Fråga**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalära uttryck
Dessutom tooproperty refererar till, hello SELECT-satsen stöder också skaläruttryck konstanter, aritmetiska uttryck, logiska uttryck osv. Här är till exempel en enkel ”Hello World”-fråga.

**Fråga**

    SELECT "Hello World"

**Resultat**

    [{
      "$1": "Hello World"
    }]


Här är ett mer avancerat exempel som använder ett skalärt uttryck.

**Fråga**

    SELECT ((2 + 11 % 7)-2)/3    

**Resultat**

    [{
      "$1": 1.33333
    }]


I följande exempel hello, är hello resultatet av hello skalära uttrycket ett booleskt värde.

**Fråga**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

**Resultat**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Skapa en objekt och matris
En annan nyckelfunktion i DocumentDB API SQL är array-objekt skapas. Observera att vi har skapat ett nytt JSON-objekt i föregående exempel hello. På samma sätt kan kan en också skapa matriser som visas i följande exempel hello:

**Fråga**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

**Resultat**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a id="ValueKeyword"></a>VÄRDET nyckelord
Hej **värdet** nyckelordet ger ett sätt tooreturn JSON-värde. Till exempel hello frågan nedan returnerar hello skalära `"Hello World"` i stället för `{$1: "Hello World"}`.

**Fråga**

    SELECT VALUE "Hello World"

**Resultat**

    [
      "Hello World"
    ]


hello följande fråga returnerar hello JSON-värde utan hello `"address"` etikett i hello resultat.

**Fråga**

    SELECT VALUE f.address
    FROM Families f    

**Resultat**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

hello följande exempel utökar den här tooshow hur tooreturn JSON primitiva värden (hello lövnivån av hello JSON-träd). 

**Fråga**

    SELECT VALUE f.address.state
    FROM Families f    

**Resultat**

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a>* Operatorn
hello särskilda operatorn (*) är stöds tooproject hello dokumentet-är. När den används, måste det vara hello endast beräknade fält. När en fråga som `SELECT * FROM Families f` är giltigt, `SELECT VALUE * FROM Families f ` och `SELECT *, f.id FROM Families f ` är inte giltiga.

**Fråga**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Resultat**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

### <a id="TopKeyword"></a>Operatorn TOP
hello ÖVERSTA nyckelord kan vara används toolimit hello antal värden från en fråga. När upp används tillsammans med hello ORDER BY-sats, är hello resultatuppsättningen begränsad toohello N första beställda värden. Annars returneras hello N första resultat i en odefinierad ordning. Som bästa praxis, i en SELECT-instruktion alltid Använd en ORDER BY-sats med hello TOP-instruktion. Detta är hello endast sätt toopredictably anger vilka rader som påverkas av ÖVERKANT. 

**Fråga**

    SELECT TOP 1 * 
    FROM Families f 

**Resultat**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

TOP kan användas med ett konstant värde (som visas ovan) eller med ett variabelvärde med hjälp av frågor med parametrar. Mer information finns i parameterfrågor nedan.

### <a id="Aggregates"></a>Mängdfunktioner
Du kan också utföra aggregeringar i hello `SELECT` satsen. Mängdfunktioner utför en beräkning på en uppsättning värden och returnera ett enstaka värde. Till exempel returnerar hello följande fråga hello antal family dokument inom hello samlingen.

**Fråga**

    SELECT COUNT(1) 
    FROM Families f 

**Resultat**

    [{
        "$1": 2
    }]

Du kan också returnera hello skalära värde hello sammanställd med hjälp av hello `VALUE` nyckelord. Till exempel returnerar hello följande fråga hello antalet värden som ett tal:

**Fråga**

    SELECT VALUE COUNT(1) 
    FROM Families f 

**Resultat**

    [ 2 ]

Du kan också utföra aggregeringar i kombination med filter. Till exempel returnerar hello följande fråga hello antal dokument med hello-adress i hello delstaten Washington.

**Fråga**

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

**Resultat**

    [ 1 ]

hello listas följande tabell hello stöds mängdfunktioner i DocumentDB-API. `SUM`och `AVG` utförs via numeriska värden, medan `COUNT`, `MIN`, och `MAX` kan utföras via siffror, strängar, booleska värden och null-värden. 

| Användning | Beskrivning |
|-------|-------------|
| ANTAL | Returnerar hello antal objekt i hello uttryck. |
| SUMMA   | Returnerar hello summan av alla hello-värden i hello uttryck. |
| MIN   | Returnerar hello minimivärdet i hello uttryck. |
| MAX   | Returnerar hello maxvärdet i hello uttryck. |
| GENOMSN.   | Returnerar hello medelvärdet av hello i hello uttryck. |

Aggregeringar kan också utföras via hello resultaten av en matris iteration. Mer information finns i [matris Iteration i frågor](#Iteration).

> [!NOTE]
> När du använder hello Azure portal Frågeutforskaren, Observera att aggregering frågor kan returnera hello delvis sammanlagda resultat via en fråga sida. hello SDK producerar en enda ackumulerade värdet på alla sidor. 
> 
> I ordning tooperform aggregering frågor med kod, som du behöver .NET SDK 1.12.0, .NET Core SDK 1.1.0 eller Java SDK 1.9.5 eller senare.    
>

## <a id="OrderByClause"></a>ORDER BY-sats
Som i ANSI-SQL, kan du använda en valfri Order By-sats samtidigt som frågor körs. hello-sats kan innehålla en valfri ASC/DESC argumentet toospecify hello order som resultat måste hämtas.

Här är till exempel en fråga som hämtar familjer i den ordning de hello resident stad namn.

**Fråga**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

**Resultat**

    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]

Här är en fråga som hämtar familjer efter skapandedatum som lagras som ett tal som representerar hello epok tid, dvs, förfluten tid sedan den 1 januari 1970 i sekunder.

**Fråga**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

**Resultat**

    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]

## <a id="Advanced"></a>Avancerade begrepp och SQL-frågor

### <a id="Iteration"></a>Upprepning
En ny konstruktion har lagts till via hello **IN** nyckelord i DocumentDB API SQL tooprovide stöd för att iterera över JSON-matriser. Hej från datakällan har stöd för iteration. Vi börjar med hello följande exempel:

**Fråga**

    SELECT * 
    FROM Families.children

**Resultat**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Nu ska vi titta på en annan fråga som utför iteration över underordnade i hello samling. Observera hello skillnaden i hello utdata matris. Det här exemplet delar upp `children` och förenklas hello resultat i en matris.  

**Fråga**

    SELECT * 
    FROM c IN Families.children

**Resultat**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Detta kan vara mer används toofilter på varje enskild post hello matrisen som visas i följande exempel hello:

**Fråga**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Resultat**  

    [{
      "givenName": "Lisa"
    }]

Du kan också utföra sammanställning över hello resultatet av matrisen iteration. Till exempel räknar hello följande fråga hello antalet underordnade objekt mellan alla serier.

**Fråga**

    SELECT COUNT(child) 
    FROM child IN Families.children

**Resultat**  

    [
      { 
        "$1": 3
      }
    ]

### <a id="Joins"></a>Kopplingar
Hello måste toojoin över tabeller är viktigt i en relationsdatabas. Dess hello logiska naturlig följd toodesigning normaliserade scheman. Motstridiga toothis, DocumentDB API behandlar hello Avnormaliserade datamodellen schemafria dokument. Detta är hello logiska motsvarigheten till en ”självkoppling”.

hello-syntax som hello språket stöder är < from_source1 > < from_source2 > Anslut till koppling... Anslut < from_sourceN >. Generellt sett detta returnerar en uppsättning **N**- tupplar (tuppel med **N** värden). Varje tuppel har värden som genereras av alla samling alias iterera över sina respektive uppsättningar. Detta är med andra ord en fullständig kryssprodukten av hello mängder som deltar i hello koppling.

hello följande exempel visar hur det fungerar hello JOIN-sats. I följande exempel hello, kan hello resultatet är tom eftersom hello kryssprodukten av varje dokument från käll- och tomma är tom.

**Fråga**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Resultat**  

    [{
    }]


I följande exempel hello, är hello koppling mellan hello dokumentroten och hello `children` subroot. Det är en kryssprodukten mellan två JSON-objekt. hello faktum att underordnade är en matris är inte effektivt i hello koppling eftersom vi arbetar med en enda rot är hello underordnade matris. Därför innehåller hello resultatet bara två resultat eftersom hello kryssprodukten av varje dokument med hello matris ger exakt bara ett dokument.

**Fråga**

    SELECT f.id
    FROM Families f
    JOIN f.children

**Resultat**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


hello som följande exempel visar en mer konventionella koppling:

**Fråga**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Resultat**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



hello först öppna toonote är att hello `from_source` av hello **ansluta** -satsen är en iterator. Därför hello flödet är i det här fallet följande:  

* Expandera varje element i underordnade **c** i hello matris.
* Tillämpa en kryssprodukten med hello hello dokumentets rot **f** med varje underordnat element **c** som har förenklad i hello första steget.
* Slutligen projektet hello rotobjektet **f** namnegenskap enbart. 

hello första dokumentet (`AndersenFamily`) innehåller endast ett underordnat element, så hello resultatet innehåller bara ett enda objekt motsvarande toothis dokument. hello andra dokumentet (`WakefieldFamily`) innehåller två underordnade. Därför genererar hello kryssprodukten ett separat objekt för varje underordnad, vilket ledde till två objekt, en för varje underordnad motsvarande toothis dokument. hello rot fält i båda dessa dokument är hello detsamma, precis som du förväntar dig i en kryssprodukten.

hello riktigt utility av hello koppling är tooform tupplar från hello-kryssprodukten i en form som annars svårt tooproject. Som vi se i hello exemplet nedan kan du dessutom kunde filtrera på hello kombination av en tuppel som kan hello användaren har valt ett villkor uppfylls av hello tupplar övergripande.

**Fråga**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

**Resultat**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



Det här exemplet är en naturlig förlängning av hello föregående exempel och utför en dubbel koppling. Därför kan hello kryssprodukten visas som hello följande genererat kod:

    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`har ett underordnat objekt som har en husdjur. Därför hello kryssprodukten ger en rad (1\*1\*1) från den här serien. WakefieldFamily men har två underordnade objekt, men endast ett underordnat objekt ”Jesse” har husdjur. Jesse har två husdjur om. Därför hello kryssprodukten ger 1\*1\*2 = 2 rader från den här serien.

I nästa exempel hello finns ett filter på `pet`. Detta omfattar inte alla hello tupplar där hello husdjur namn inte är ”skuggkopia”. Observera att vi kan toobuild tupplar från matriser, filter på hello-element för hello tuppel och projektet valfri kombination av hello-element. 

**Fråga**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Resultat**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a id="JavaScriptIntegration"></a>JavaScript-integrering
Azure Cosmos-DB är en programmeringsmodell för att köra JavaScript-baserade programlogik direkt på hello samlingar som lagrade procedurer och utlösare. Detta möjliggör både:

* Möjlighet-toodo högpresterande transaktionella CRUD-åtgärder och frågor mot dokument i en samling tack vare hello djupgående integration av JavaScript-körning direkt i databasmotorn hello. 
* En fysisk modellering av Kontrollflöde, variabel omfång och tilldelning och integration av undantagshantering primitiver med databastransaktioner. Mer information om Azure DB som Cosmos-stöd för JavaScript-integrering finns toohello JavaScript programmering av serversidan dokumentation.

### <a id="UserDefinedFunctions"></a>Användardefinierade funktioner (UDF)
DocumentDB API SQL ger stöd för användaren definierat funktioner (UDF) tillsammans med hello-typer som har redan definierats i den här artikeln. I synnerhet stöds skalära UDF: er där hello utvecklare kan skicka in noll eller flera argument och returnera ett enda argument resultat tillbaka. Var och en av de här argumenten kontrolleras för att vara giltiga JSON-värdena.  

Hej DocumentDB API SQL-syntax är utökat toosupport anpassad programlogik med hjälp av dessa användardefinierade funktioner. UDF: er kan registreras med DocumentDB-API och sedan refereras som en del av en SQL-fråga. Faktum är utformad hello UDF: er är exquisitely toobe som anropas av frågor. Som en naturlig följd toothis alternativ UDF: er har inte åtkomst toohello context-objektet som hello andra JavaScript typer (lagrade procedurer och utlösare) har. Eftersom frågor körs i skrivskyddat läge kan köras de på primära eller sekundära repliker. Därför är UDF: er utformad toorun på sekundära repliker till skillnad från andra typer av JavaScript.

Nedan visas ett exempel på hur en UDF kan registreras på hello Cosmos-DB-databas, speciellt under en dokumentsamling.

       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  

hello exemplet ovan skapar en UDF-fil som heter `REGEX_MATCH`. Den accepterar två strängvärden som JSON `input` och `pattern` och kontrollerar om hello första matchar hello mönster som anges i andra hello använder Javascript's string.match()-funktionen.

Vi kan nu använda den här UDF i en fråga i en projektion. UDF: er måste vara kvalificerat med hello skiftlägeskänsliga prefixet ”udf”. När anropas från frågor. 

> [!NOTE]
> Tidigare too3/17/2015, Cosmos DB stöds UDF anrop utan hello ”udf”. prefix som väljer REGEX_MATCH(). Det här anropa mönstret är föråldrad.  
> 
> 

**Fråga**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Resultat**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

hello UDF kan också användas i ett filter som visas i hello exemplet nedan är också kvalificerad med hello ”udf”. prefix:

**Fråga**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Resultat**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


I princip UDF: er är giltig skalära uttryck och kan användas i både projektioner och filter. 

tooexpand på hello kraften i UDF: er, ska vi titta på ett annat exempel med villkorlig logik:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);


Nedan finns ett exempel som att övningarna hello UDF.

**Fråga**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

**Resultat**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Eftersom hello föregående exempel samlade integreras UDF: er hello kraften i JavaScript-språket med hello DocumentDB API SQL tooprovide en omfattande programmerbara gränssnitt toodo komplex procedurmässig, villkorlig logik med hello hjälp av inbyggda JavaScript-körning funktioner.

DocumentDB API SQL här hello argument toohello UDF: er för varje dokument i hello källan hello aktuell etapp (WHERE-satsen eller SELECT-satsen) av bearbetning hello UDF. hello resultatet är integrerat i hello övergripande körningspipeline sömlöst. Om hello egenskaper som anges tooby hello UDF parametrar inte är tillgängliga i hello JSON-värde hello parametern betraktas som Odefinierad och därför hello ignoreras helt UDF-anrop. På liknande sätt om hello resultatet av hello UDF är odefinierad ingår den inte i hello resultat. 

Sammanfattningsvis är UDF: er bra verktyg toodo komplicerad affärslogik som en del av hello frågan.

### <a name="operator-evaluation"></a>Operatorn utvärdering
Cosmos DB, ritar bredd med JavaScript-operatörer och dess utvärdering semantik hello miljöpåverkan som JSON-databas. Medan Cosmos DB försöker toopreserve JavaScript-semantik i JSON-support, avviker hello åtgärden utvärdering i vissa fall.

I DocumentDB API SQL, är till skillnad från i traditionella SQL hello typer av värden ofta inte känd tills hello värden hämtas från databasen. Köra frågor i ordning tooefficiently, de flesta av hello operatörer har strikt krav. 

DocumentDB API SQL utförs inte implicita konverteringar, till skillnad från JavaScript. Till exempel en fråga som `SELECT * FROM Person p WHERE p.Age = 21` matchar dokument som innehåller en ålder egenskap vars värde är 21. Andra dokument vars ålder-egenskap stämmer med strängen ”21” eller andra eventuellt oändlig variationer som ”021”, ”21.0”, ”0021”, ”00021”, etc. matchas inte. Detta är däremot toohello JavaScript där hello strängvärden är implicit omvandlas toonumbers (baserat på operator, ex: ==). Det här alternativet är avgörande för effektiv index som matchar i DocumentDB API SQL. 

## <a name="parameterized-sql-queries"></a>Parametriserade SQL-frågor
Cosmos DB stöder frågor med parametrar uttryckt med hello bekant @ notation. Parametriserade SQL ger stabil hantering och undantagstecken användarindata, förhindra oavsiktlig exponering av data via SQL injection. 

Du kan till exempel skriva en fråga som tar efternamn och region som parametrar och sedan exekverar för olika värden för efternamn och adressläge baserat på indata från användaren.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Denna begäran kan skicka tooCosmos DB som en JSON-fråga som visas nedan.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

hello argumentet tooTOP kan anges med frågor med parametrar som visas nedan.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parametervärden kan vara en giltig JSON (strängar, siffror, booleska värden, null, även matriser eller kapslade JSON). Även eftersom Cosmos-DB-schema mindre, valideras parametrar inte mot alla typer.

## <a id="BuiltinFunctions"></a>Inbyggda funktioner
Cosmos DB stöder också ett antal inbyggda funktioner för vanliga åtgärder som kan användas i frågor som användardefinierade funktioner (UDF).

| Funktionen grupp          | Åtgärder                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Matematiska funktioner  | ABS, TAKET, EXP, VÅNING, LOG, LOG10, POWER, runda, LOGGA, SQRT, ruta, AVKORTA, ARCCOS, ARCSIN, ARCTAN, ATN2, COS, bet, grader, PI, radianer, SIN och TAN |
| Ange kontrollerar funktioner | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED och IS_PRIMITIVE                                                           |
| Strängfunktioner        | CONCAT, innehåller, ENDSWITH, INDEX_OF, vänster, längd, nedre, LTRIM, Ersätt, REPLIKERA, OMVÄND, höger, RTRIM, STARTSWITH, DELSTRÄNGEN och övre       |
| Matrisfunktioner         | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH och ARRAY_SLICE                                                                                         |
| Spatial funktioner       | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID och ST_ISVALIDDETAILED                                                                           | 

Om du använder en användardefinierad funktion (UDF) som en inbyggd funktion är nu tillgänglig, bör du använda hello motsvarande inbyggd funktion som det ska toobe snabbare toorun och mer effektivt. 

### <a name="mathematical-functions"></a>Matematiska funktioner
hello matematiska funktioner varje utför en beräkning, baserat på värden som har angetts som argument och returnerar ett numeriskt värde. Här är en tabell med inbyggda matematiska funktioner som stöds.


| Användning | Beskrivning |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [[ABS (num_expr)](#bk_abs) | Returnerar hello absolut (positivt) värdet för hello angetts numeriskt uttryck. |
| [TAKET (num_expr)](#bk_ceiling) | Returnerar hello minsta heltal som är större än eller lika med för att hello uttryck. |
| [VÅNING (num_expr)](#bk_floor) | Returnerar hello största heltal som är mindre än eller lika toohello angivna numeriska uttrycket. |
| [EXP (num_expr)](#bk_exp) | Returnerar hello e upphöjt till hello angivna numeriska uttrycket. |
| [LOGGEN (num_expr [, base])](#bk_log) | Returnerar hello naturliga logaritmen för hello angetts numeriskt uttryck eller hello logaritmen med hello angetts bas |
| [Log10 (num_expr)](#bk_log10) | Returnerar hello 10-logaritmiska värdet för hello angetts numeriskt uttryck. |
| [AVRUNDA (num_expr)](#bk_round) | Returnerar ett numeriskt värde avrundade toohello närmaste heltal. |
| [AVKORTA (num_expr)](#bk_trunc) | Returnerar ett numeriskt värde, trunkerat toohello närmaste heltal. |
| [SQRT (num_expr)](#bk_sqrt) | Returnerar hello kvadratroten ur hello angivna numeriska uttrycket. |
| [RUTA (num_expr)](#bk_square) | Returnerar hello rektangulär hello angivna numeriska uttrycket. |
| [STRÖMFÖRSÖRJNING (num_expr, num_expr)](#bk_power) | Returnerar hello kraften hos hello angetts stränguttryck toohello värde har angetts. |
| [LOGGA (num_expr)](#bk_sign) | Returnerar hello logga värdet (-1, 0, 1) för hello angetts numeriskt uttryck. |
| [ARCCOS (num_expr)](#bk_acos) | Returnerar hello vinkel angivna i radianer, vars cosinus är hello numeriska uttrycket; kallas även cosinus. |
| [ARCSIN (num_expr)](#bk_asin) | Returnerar hello vinkel angivna i radianer, vars sinus är hello numeriska uttrycket. Detta kallas också arcsinus. |
| [ARCTAN (num_expr)](#bk_atan) | Returnerar hello vinkel angivna i radianer, vars tangens är hello numeriska uttrycket. Detta kallas också tangens. |
| [ATN2 (num_expr)](#bk_atn2) | Returnerar hello vinkel i radianer mellan hello positiva x-axeln och hello ray från hello toohello origo (y, x), där x och y är hello värdena för hello två angivna float uttryck. |
| [COS (num_expr)](#bk_cos) | Returnerar hello trigonometriska värden cosinus för hello angiven vinkel i radianer, i hello angivna uttrycket. |
| [BET (num_expr)](#bk_cot) | Returnerar hello trigonometriska värden cotangens för hello angiven vinkel i radianer, i hello anges numeriskt uttryck. |
| [GRADER (num_expr)](#bk_degrees) | Returnerar hello motsvarande vinkel i grader för en vinkel angiven i radianer. |
| [PI)](#bk_pi) | Returnerar hello konstantvärde pi. |
| [RADIANER (num_expr)](#bk_radians) | Returnerar radianer när ett numeriskt uttryck i grader anges. |
| [SIN (num_expr)](#bk_sin) | Returnerar hello trigonometriska värden sinus för hello angiven vinkel i radianer, i hello angivna uttrycket. |
| [TAN (num_expr)](#bk_tan) | Returnerar hello tangens för hello inkommande uttryck i hello angivna uttrycket. |

Du kan till exempel nu köra frågor som hello nedan:

**Fråga**

    SELECT VALUE ABS(-4)

**Resultat**

    [4]

hello största skillnaden mellan Cosmos DB funktioner jämfört med tooANSI SQL är att de är utformad toowork bra med schemat mindre och blandad schemadata. Till exempel om du har ett dokument där egenskapen hello saknas eller har ett icke-numeriska värde som ”okänt” och sedan hello dokumentet hoppas över, i stället för att returnera ett fel.

### <a name="type-checking-functions"></a>Ange kontrollerar funktioner
hello typ kontrollerar funktioner kan du toocheck hello typ av ett uttryck i SQL-frågor. Typen som kontrollerar funktioner kan inte användas toodetermine hello typ av egenskaper i dokument på hello direkt när den variabel eller okänd. Här är en tabell med stöds inbyggd typ kontrollerar funktioner.

<table>
<tr>
  <td><strong>Användning</strong></td>
  <td><strong>Beskrivning</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om hello typ av hello värde är en matris.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om hello typ av hello värde är ett booleskt värde.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om hello typ av hello värde är null.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om hello typ av hello värde är ett tal.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om hello typ av hello värde är ett JSON-objekt.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om hello typ av hello värde är en sträng.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (uttryck)</a></td>
  <td>Returnerar ett booleskt värde som anger om hello typ av hello värde är en sträng, siffra, Boolean eller null.</td>
</tr>

</table>

Med dessa funktioner kan köra du nu frågor som hello nedan:

**Fråga**

    SELECT VALUE IS_NUMBER(-4)

**Resultat**

    [true]

### <a name="string-functions"></a>Strängfunktioner
hello följande skalärfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde. Här är en tabell med inbyggda strängfunktioner:

| Användning | Beskrivning |
| --- | --- |
| [LÄNGDEN (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |Returnerar hello antalet tecken i hello angetts stränguttryck |
| [SAMMANFOGA (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat) |Returnerar en sträng som är hello resultat sammanfoga två eller flera strängvärden. |
| [DELSTRÄNGEN (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |Returnerar en del av ett stränguttryck. |
| [STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra |
| [ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra |
| [INNEHÅLLER (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |Returnerar ett booleskt värde som anger om hello första stränguttryck innehåller hello andra. |
| [INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |Returnerar hello startposition för hello första förekomsten av hello andra stränguttryck inom hello första angivet stränguttryck eller -1 om hello strängen inte hittas. |
| [VÄNSTER (str_expr num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |Returnerar hello vänstra del av en sträng med hello angivna antalet tecken. |
| [RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |Returnerar hello just en del av en sträng med hello angivet antal tecken. |
| [LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |Returnerar ett stränguttryck när den tar bort inledande blanksteg. |
| [RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |Returnerar ett stränguttryck efter trunkering av alla avslutande blanksteg. |
| [NEDRE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |Returnerar ett stränguttryck efter konvertering versal data toolowercase. |
| [ÖVRE (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |Returnerar ett stränguttryck efter konvertering gemen data toouppercase. |
| [Ersätt (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |Ersätter alla förekomster av ett angivet strängvärde med ett annat värde. |
| [REPLIKERA (str_expr, num_expr)](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |Upprepar ett strängvärde till ett angivet antal gånger. |
| [OMVÄND (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |Returnerar hello omvänd ordning i ett strängvärde. |

Med dessa funktioner kan köra du nu frågor som hello nedan. Exempelvis returnerar hello efternamn versaler på följande sätt:

**Fråga**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Resultat**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Eller sammanfoga strängar som i det här exemplet:

**Fråga**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Resultat**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Strängfunktioner kan också användas i hello där satsen toofilter resultat, precis som i följande exempel hello:

**Fråga**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Resultat**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Matrisfunktioner
hello följande skalärfunktioner utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde. Här är en tabell med inbyggda matrisfunktioner:

| Användning | Beskrivning |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |Returnerar hello antalet element i hello angetts matrisuttryck. |
| [ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat) |Returnerar en matris som är hello resultat sammanfoga två eller flera matrisvärden. |
| [ARRAY_CONTAINS (arr_expr uttryck [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains) |Returnerar ett booleskt värde som anger om hello matris innehåller hello anges värdet. Ange om hello matchning är fullständigt eller partiellt. |
| [ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice) |Returnerar en del av ett matrisuttryck. |

Matrisfunktioner kan inte använda toomanipulate matriser i JSON. Här är till exempel en fråga som returnerar alla dokument där en av hello överordnade är ”Robin Wakefield”. 

**Fråga**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Resultat**

    [{
      "id": "WakefieldFamily"
    }]

Du kan ange ett partiellt fragment för matchande element inom hello matris. hello följande exempelfråga hittar du alla överordnade med hello `givenName` av `Robin`.

**Fråga**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

**Resultat**

    [{
      "id": "WakefieldFamily"
    }]


Här är ett annat exempel som använder ARRAY_LENGTH tooget hello antalet underordnade per familj.

**Fråga**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Resultat**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Spatial funktioner
Cosmos DB stöder hello följande öppna geospatiala Consortium (OGC) inbyggda funktioner för geospatial frågor. 

<table>
<tr>
  <td><strong>Användning</strong></td>
  <td><strong>Beskrivning</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Returnerar hello avståndet mellan hello två GeoJSON punkt, Polygon eller LineString uttryck.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Returnerar ett booleskt uttryck som anger om hello första GeoJSON objekt (Point, LineString eller Polygon) är i hello andra GeoJSON objekt (Point, LineString eller Polygon).</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Returnerar ett booleskt uttryck som anger om intersect hello två angivna GeoJSON-objekt (Point, LineString eller Polygon).</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Returnerar ett booleskt värde som anger om hello angiven GeoJSON punkt, Polygon eller LineString uttryck är giltig.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Returnerar ett JSON-värde som innehåller ett booleskt värde om hello angivna GeoJSON punkt, Polygon eller LineString uttrycket är giltig och om det är ogiltig, dessutom hello orsak som ett strängvärde.</td>
</tr>
</table>

Spatial funktioner kan inte använda tooperform närhet frågor mot spatialdata. Här är till exempel en fråga som returnerar alla familj dokument att 30 km av hello angivna platsen använder hello ST_DISTANCE inbyggd funktion. 

**Fråga**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Resultat**

    [{
      "id": "WakefieldFamily"
    }]

Mer information om geospatiala stöds i Cosmos-databasen finns [arbeta med geospatiala data i Azure Cosmos DB](geospatial.md). Som packar upp spatial funktioner och hello SQL-syntax för Cosmos DB. Nu ska vi ta en titt på hur LINQ-frågor fungerar och hur den interagerar med hello syntax vi sett hittills.

## <a id="Linq"></a>LINQ tooDocumentDB API SQL
LINQ är en programmeringsmodell för .NET som representerar beräkning som frågor för dataströmmar med objekt. Cosmos DB tillhandahåller en klientsidan biblioteket toointerface med LINQ genom att underlätta konvertering mellan JSON och .NET-objekt och mappning från en delmängd av LINQ frågor tooCosmos DB frågor. 

hello bilden nedan visar hello-arkitektur stöder LINQ-frågor med Cosmos DB.  Med hello Cosmos-DB-klient kan utvecklare skapa ett **IQueryable** objekt att direkt frågor hello fråga Cosmos-DB-provider, som sedan översätter hello LINQ-fråga till en Cosmos-DB-fråga. hello fråga skickas sedan toohello Cosmos DB server tooretrieve en uppsättning resultat i JSON-format. hello returnerade resultatet är avserialiserat till en dataström med .NET-objekt på hello på klientsidan.

![Arkitektur för LINQ-frågor med DocumentDB-API: T - SQL-syntaxen, JSON-frågespråket, databasbegrepp och SQL-frågor][1]

### <a name="net-and-json-mapping"></a>.NET och JSON-mappning
hello mappning mellan .NET-objekt och JSON-dokument är naturlig - varje medlem datafält mappas tooa JSON-objekt, där hello fältnamnet är mappad toohello ”nyckeln” en del av hello objekt och hello ”värde” del tillhör rekursivt mappas toohello värdet hello-objektet. Överväg följande exempel hello: hello familj objekt som skapas är mappade toohello JSON-dokumentet som visas nedan. Och vice versa hello JSON-dokumentet är mappade tillbaka tooa .NET-objekt.

**C#-klass**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-toosql-translation"></a>LINQ tooSQL översättning
Hej Cosmos frågan databasleverantör utför en bästa prestanda-mappning från en LINQ-fråga till en Cosmos-Databasens SQL-fråga. I följande beskrivning hello, förutsätter vi att du hello läsaren har en grundläggande kunskaper inom LINQ.

Vi kan först stöder alla JSON primitiva typer – numeriska typer, boolean, sträng eller null för hello typsystemet. Endast typerna JSON stöds. följande skaläruttryck hello stöds.

* Konstanta värden – dessa inkluderar konstanta värden hello primitiva datatyper för närvarande hello hello frågan utvärderas.
* Egenskapen/matrisen indexuttryck – dessa uttryck finns toohello-egenskapen för ett objekt eller ett matriselement.
  
     familj. ID;    Family.children[0].familyName;    Family.children[0].grade;    Family.children[n].grade; n är en int-variabel
* Aritmetiska uttryck - dessa inkluderar vanliga aritmetiska uttryck på numeriska och booleska värden. Hello fullständig lista finns i toohello SQL-specifikationen.
  
     2 * family.children[0].grade;    x + y;
* Jämförelse av stränguttryck - bland annat att jämföra ett värde toosome konstantsträng strängvärde.  
  
     mother.familyName == ”Smith”;    child.givenName == s. s är en string-variabel
* / Objektmatris skapa uttryck - uttrycken returnerade ett objekt av sammansatta värde eller en anonym typ eller en matris med sådana objekt. Dessa värden kan vara kapslade.
  
     ny överordnad {familyName = ”Smith”, givenName = ”Johan”}; nya {först = 1, andra = 2}; en anonym typ med två fält              
     nya int [] {3, child.grade, 5};

### <a id="SupportedLinqOperators"></a>Lista över stöds LINQ-operatorer
Här är en lista över stöds LINQ operatorer i hello LINQ-providern ingår i hello .NET DocumentDB SDK.

* **Välj**: projektioner översätta toohello SQL väljer inklusive objektkonstruktion
* **Där**: filter översätta toohello SQL WHERE och stöd för översättningen mellan & &, || och! toohello SQL-operatörer
* **SelectMany**: tillåter återgång av matriser toohello SQL JOIN-satsen. Kan använda toochain/kapsla uttryck toofilter på matriselement
* **OrderBy och OrderByDescending**: översätter tooORDER BY stigande/fallande
* **Antal**, **summan**, **Min**, **Max**, och **genomsnittlig** operatorer för aggregering och motsvarigheterna asynkrona **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, och **AverageAsync**.
* **CompareTo**: översätter toorange jämförelser. Används vanligtvis för strängar eftersom de inte jämförbar i .NET
* **Ta**: översätter toohello SQL upp för att begränsa resultaten av en fråga
* **Matematiska funktioner**: har stöd för översättning av. NET'S Abs ARCCOS, ARCSIN ARCTAN, taket, Cos, Exp, våning, Log, Log10, Pow, runda, logga, Sin, Sqrt, Tan, trunkera toohello motsvarande SQL inbyggda funktioner.
* **Sträng funktioner**: har stöd för översättning av. NET'S Concat innehåller EndsWith, IndexOf, Count, ToLower, TrimStart, Ersätt, omvänd, TrimEnd, StartsWith, understrängen ToUpper toohello motsvarande SQL inbyggda funktioner.
* **Matrisen funktioner**: har stöd för översättning av. NET'S Concat och innehåller antalet toohello motsvarande SQL inbyggda funktioner.
* **Tilläggsfunktioner geospatiala**: har stöd för översättning av stub-metoder avståndet i IsValid, och IsValidDetailed toohello motsvarande SQL inbyggda funktioner.
* **Användardefinierade funktionen tilläggsfunktionen**: har stöd för översättning av hello stub-metoden UserDefinedFunctionProvider.Invoke toohello motsvarande användardefinierad funktion.
* **Diverse**: har stöd för översättning av hello slå samman och villkorlig operatörer. Kan översätta innehåller tooString innehåller, ARRAY_CONTAINS eller hello SQL IN beroende på kontext.

### <a name="sql-query-operators"></a>SQL-frågeoperatorer
Här följer några exempel som visar hur vissa hello standard LINQ frågeoperatorer översätts ned tooCosmos DB frågor.

#### <a name="select-operator"></a>Välj Operator
hello-syntaxen är `input.Select(x => f(x))`, där `f` är ett skalärt uttryck.

**LINQ lambda-uttryck**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda-uttryck**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda-uttryck**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operator
hello-syntaxen är `input.SelectMany(x => f(x))`, där `f` är ett skalärt uttryck som returnerar en Mängdtyp.

**LINQ lambda-uttryck**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Där operator
hello-syntaxen är `input.Where(x => f(x))`, där `f` är ett skalärt uttryck som returnerar ett booleskt värde.

**LINQ lambda-uttryck**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda-uttryck**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Sammansatta SQL-frågor
hello ovan operatörer kan vara bestå tooform kraftfullare frågor. Eftersom Cosmos DB stöder kapslade samlingar, kan hello sammansättning vara antingen sammanfogas eller kapslade.

#### <a name="concatenation"></a>Sammanfogning
hello-syntaxen är `input(.|.SelectMany())(.Select()|.Where())*`. En sammanfogad fråga kan börja med en valfri `SelectMany` frågan följt av flera `Select` eller `Where` operatörer.

**LINQ lambda-uttryck**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda-uttryck**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda-uttryck**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda-uttryck**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>För många kapslade
hello-syntaxen är `input.SelectMany(x=>x.Q())` där Q är en `Select`, `SelectMany`, eller `Where` operator.

I en kapslad fråga är hello inre frågan tillämpade tooeach elementet för yttre mängden hello. En viktig funktion är hello inre frågan kan referera toohello fält i hello element i hello yttre mängd som Självkopplingar.

**LINQ lambda-uttryck**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda-uttryck**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda-uttryck**

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a id="ExecutingSqlQueries"></a>Kör SQL-frågor
Cosmos DB visar resurser via ett REST-API som kan anropas med valfritt språk som kan göra HTTP/HTTPS-förfrågningar. Dessutom erbjuder Cosmos DB programmeringsbibliotek för flera populära språk som .NET, Node.js, JavaScript och Python. hello REST-API och hello stöder olika bibliotek alla frågor via SQL. hello .NET SDK stöder LINQ frågar dessutom tooSQL.

Hej följande exempel visas hur toocreate en fråga och skicka den mot en Cosmos-DB-databaskonto.

### <a id="RestAPI"></a>REST-API
Cosmos DB erbjuder en öppen RESTful-programmeringsmiljö via HTTP. Databasen konton kan etableras med hjälp av en Azure-prenumeration. Hej Cosmos DB resursmodell består av en uppsättning resurser under ett databaskonto som är adresserbara via en logisk och stabil URI. En uppsättning resurser är refererad tooas en feed i det här dokumentet. Ett databaskonto består av en uppsättning databaser som alla innehåller flera samlingar, var och en av vilka i sin tur innehåller dokument, UDF: er och andra typer av resurser.

hello grundläggande interaktion modellen med dessa resurser är via hello HTTP-verb GET, PUT, POST och DELETE med sina standard tolkning. hello POST verb används för att skapa en ny resurs, för att köra en lagrad procedur eller för att utfärda en Cosmos-DB-fråga. Frågor är alltid skrivskyddade åtgärder med inga sidoeffekter.

hello visar följande exempel en POST för en DocumentDB-API-fråga som görs mot en samling som innehåller hello två exempeldokument vi har granskat hittills. hello-frågan har ett enkelt filter på hello JSON namnegenskapen. Observera hello användning av hello `x-ms-documentdb-isquery` och Content-Type: `application/query+json` huvuden toodenote som hello åtgärden är en fråga.

**Förfrågan**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


**Resultat**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


hello andra exemplet visar en mer komplex fråga som returnerar flera resultat från hello koppling.

**Förfrågan**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Resultat**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Om en frågas resultat får inte plats i en enskild sida med resultat hello REST-API och returnerar sedan en fortsättningstoken via hello `x-ms-continuation-token` Svarsrubrik. Klienter kan sidbryta resultat genom att inkludera hello-huvud i efterföljande resultat. hello antalet resultat per sida kan också kontrolleras via hello `x-ms-max-item-count` nummer sidhuvud. Om hello angivna frågan har en Aggregeringsfunktion som `COUNT`, och sedan hello fråga sida kan returnera en delvis insamlat värde hello sida med resultat. hello-klienter måste utföra en andra nivå sammanställning över dessa resultat tooproduce hello slutliga resultat, till exempel, sum över hello antal returneras i hello enskilda sidor tooreturn hello totala antalet.

toomanage hello data konsekvent princip för frågor, Använd hello `x-ms-consistency-level` huvud som alla REST API-begäranden. Sessionskonsekvens, är det obligatoriska tooalso echo hello senaste `x-ms-session-token` Cookie-huvud i hello-fråga. hello kan efterfrågade samling indexprincip även påverka hello konsekvenskontroll av frågeresultatet. Med hello standard indexering principinställningar för samlingar är hello index alltid aktuell med hello dokumentinnehåll och fråga resultatet överensstämmer hello konsekvenskontroll valt för data. Om hello indexering princip Avslappnad tooLazy kan frågor returnera inaktuella resultat. Mer information finns i [Azure Cosmos DB Konsekvensnivåer][consistency-levels].

Om hello konfigurerad indexprincip på hello samling inte stöder hello angivna frågan, returnerar 400 ”Felaktig begäran” hello Azure Cosmos DB-server. Returneras för intervallet frågor mot sökvägar som konfigurerats för sökningar hash (likhetsfrågor) och för sökvägar som uttryckligen är undantagen från indexering. Hej `x-ms-documentdb-query-enable-scan` huvudet kan vara angivna tooallow hello frågan tooperform en genomsökningen när ett index inte är tillgänglig.

Du kan få detaljerad mått på Frågekörningen genom att ange `x-ms-documentdb-populatequerymetrics` huvud för`True`. Mer information finns i [SQL-frågan mätvärden för Azure Cosmos DB DocumentDB API](documentdb-sql-query-metrics.md).

### <a id="DotNetSdk"></a>C# (.NET) SDK
hello .NET SDK stöder både LINQ och SQL frågor. hello följande exempel visas hur tooperform hello enkla filterfråga introduceras tidigare i det här dokumentet.

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Det här exemplet jämför två egenskaper för likhetsfrågor inom varje dokument och använder anonym projektioner. 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


hello nästa exempel visas kopplingar, uttryckt genom SelectMany för LINQ.

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



hello .NET klienten går automatiskt igenom alla hello sidor i frågeresultaten i hello foreach block som ovan. hello frågan alternativ som introduceras i hello REST API-avsnittet är också tillgängliga i hello .NET SDK med hjälp av hello `FeedOptions` och `FeedResponse` klasser i hello CreateDocumentQuery metod. hello antalet sidor som kan kontrolleras med hello `MaxItemCount` inställningen. 

Du kan också explicit styra sidindelning genom att skapa `IDocumentQueryable` med hello `IQueryable` objekt sedan genom att läsa den` ResponseContinuationToken` värden och skicka dem tillbaka som `RequestContinuationToken` i `FeedOptions`. `EnableScanInQuery`kan vara set tooenable genomsökningar när hello frågan inte stöds av hello konfigurerade indexprincip. För partitionerade samlingar kan du använda `PartitionKey` toorun hello frågan mot en enskild partition (även om Cosmos DB extraherar det automatiskt från hello frågetexten) och `EnableCrossPartitionQuery` toorun frågor som kan behöva toobe som körs mot flera partitioner. 

Se för[Azure Cosmos DB .NET-exempel](https://github.com/Azure/azure-documentdb-net) för flera sampel som innehåller frågor. 

### <a id="JavaScriptServerSideApi"></a>JavaScript API för serversidan
Cosmos DB är en programmeringsmodell för att köra JavaScript-baserade programlogik direkt på hello samlingar med lagrade procedurer och utlösare. hello JavaScript-logik som registrerats på en samlingsnivå kan sedan utfärda databasåtgärder i hello åtgärder på hello dokument av hello angivna samlingen. Dessa åtgärder är kapslas in i omgivande ACID-transaktioner.

hello följande exempel visas hur toouse hello Frågedokument i hello JavaScript server API toomake frågor från innanför lagrade procedurer och utlösare.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a id="References"></a>Referenser
1. [Introduktion tooAzure Cosmos DB][introduction]
2. [Azure Cosmos-Databasens SQL-specifikationen](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [Azure Cosmos DB .NET-exempel](https://github.com/Azure/azure-documentdb-net)
4. [Konsekvensnivåer för Azure Cosmos DB][consistency-levels]
5. ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6. JSON [http://json.org/](http://json.org/)
7. JavaScript-specifikationen [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8. LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9. Fråga utvärdering tekniker för stora databaser [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Bearbetning av frågor i parallella relationsdatabassystem, IEEE datorn Society Press, 1994
11. Lu Ooi, Tan, bearbetning av frågor i parallella relationsdatabassystem, IEEE datorn Society Press, 1994.
12. Kitts Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Report Anders Tomkins: Pig Latin: en inte så främmande språk för databehandling, SIGMOD 2008.
13. G. Graefe. hello kaskadspridas ramverk för optimering av frågan. IEEE Data Eng. Bull., 18.3: 1995.

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md