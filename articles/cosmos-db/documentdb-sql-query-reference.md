---
Rubrik: aaa ”Azure Cosmos DB DocumentDB API: SQL-syntaxen | Microsoft Docs ”beskrivning: referera till dokumentationen för hello Azure Cosmos DB DocumentDB API SQL-frågespråket.
tjänster: cosmos-db författare: mimig1 manager: jhubbard editor: mimig dokumentationcenter: ''

MS.AssetID: ms.service: cosmos-db ms.workload: data services ms.tgt_pltfrm: na ms.devlang: na ms.topic: referera ms.date: 06/13/2017 ms.author: mimig

---

# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a>Azure Cosmos DB DocumentDB API: Referens SQL-syntax

hello Azure DB-API Cosmos DocumentDB stöder förfrågningar till dokument med en bekant SQL (Structured Query Language) som grammatik via hierarkiska JSON-dokument utan explicita schema eller att sekundärindex. Det här avsnittet finns i referensdokumentationen för hello DocumentDB API SQL-frågespråket.

En genomgång av hello DocumentDB API SQL-frågespråket finns [SQL-frågor för Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).  
  
Vi också bjuda in dig toovisit hello [Query Playground](http://www.documentdb.com/sql/demo) där du kan prova Azure Cosmos DB och köra SQL-frågor mot vår datauppsättning.  
  
## <a name="select-query"></a>SELECT-frågan  
Hämtar JSON-dokument från hello-databasen. Stöder uttrycksutvärdering, projektioner filtrering och ansluter till.  hello-konventioner som används för att beskriva hello SELECT-satser är en tabell i hello Syntax konventioner avsnitt.  
  
**Syntax**  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 **Kommentarer**  
  
 Se följande avsnitt för mer information om varje sats:  
  
-   [SELECT-satsen](#bk_select_query)  
  
-   [FROM-satsen](#bk_from_clause)  
  
-   [WHERE-satsen](#bk_where_clause)  
  
-   [ORDER BY-sats](#bk_orderby_clause)  
  
hello-satser i hello SELECT-instruktion måste beställas som ovan. En av valfri hello-satser kan utelämnas. Men när valfria satser används, måste de visas i hello rätt ordning.  
  
**Logiska behandlingsordning hello SELECT-instruktion**  
  
hello är som satser bearbetas:  

1.  [FROM-satsen](#bk_from_clause)  
2.  [WHERE-satsen](#bk_where_clause)  
3.  [ORDER BY-sats](#bk_orderby_clause)  
4.  [SELECT-satsen](#bk_select_query)  

Observera att detta skiljer sig från hello ordning som de visas i hello syntax. hello ordning är så att alla nya symboler som introducerades av en bearbetade sats visas och kan användas i satser bearbetas senare. Till exempel alias som deklareras i en FROM-sats är tillgängliga i var och SELECT-satser.  

**Tecken som blanksteg och kommentarer**  

Alla blanktecken som inte är del av en sträng inom citattecken eller identifierare med citattecken är inte en del av hello språk grammatik och ignoreras vid parsning.  

kommentarer för T-SQL-format som har stöd för hello-frågespråket  

-   SQL-uttryck`-- comment text [newline]`  

Medan tecken som blanksteg och kommentarer inte har någon betydelse i hello grammatik, måste de vara används tooseparate token. Exempel: `-1e5` är ett enda nummer token, tag`: – 1 e5` följs minus token av nummer 1 och identifierare e5.  

##  <a name="bk_select_query"></a>SELECT-satsen  
hello-satser i hello SELECT-instruktion måste beställas som ovan. En av valfri hello-satser kan utelämnas. Men när valfria satser används, måste de visas i hello rätt ordning.  

**Syntax**  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 **Argument**  
  
 `<select_specification>`  
  
 Ange egenskaper eller värdet toobe som valts för hello resultat.  
  
 `'*'`  
  
Anger att hello ska hämtas utan ändringar. Mer specifikt om hello bearbetas värdet är ett objekt, hämtas alla egenskaper.  
  
 `<object_property_list>`  
  
Anger hello lista över egenskaper toobe hämtas. Varje returnerade värdet ska vara ett objekt med hello egenskaper som anges.  
  
`VALUE`  
  
Anger att hello JSON-värde ska hämtas i stället för hello fullständig JSON-objekt. Detta, till skillnad från `<property_list>` radbryts inte hello planerat värde i ett objekt.  
  
`<scalar_expression>`  
  
Uttryck som representerar hello värdet toobe beräknas. Se [skaläruttryck](#bk_scalar_expressions) information.  
  
**Kommentarer**  
  
Hej `SELECT *` syntax är bara giltigt om FROM-satsen har deklarerats exakt ett alias. `SELECT *`innehåller en identity-projektion som kan vara användbar om det behövs ingen projektion. Välj * är bara giltigt om FROM-satsen har angetts och införs bara en enda Indatakällan.  
  
Observera att `SELECT <select_list>` och `SELECT *` är ”syntaktiska socker” och kan uttryckas också med hjälp av enkla SELECT-satser som visas nedan.  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     motsvarar:  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     motsvarar:  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
**Se även**  
  
[Skalära uttryck](#bk_scalar_expressions)  
[SELECT-satsen](#bk_select_query)  
  
##  <a name="bk_from_clause"></a>FROM-satsen  
Anger hello käll- eller anslutna källor. hello FROM-satsen är valfritt. Om inte angivna, andra satser körs fortfarande som om FROM-satsen som ett enskilt dokument.  
  
**Syntax**  
  
```  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <collection_expression> [[AS] input_alias]  
        | input_alias IN <collection_expression>  
  
<collection_expression> ::=   
        ROOT   
     | collection_name  
     | input_alias  
     | <collection_expression> '.' property_name  
     | <collection_expression> '[' "property_name" | array_index ']'  
```  
  
**Argument**  
  
`<from_source>`  
  
Anger en datakälla med eller utan ett alias. Om alias inte anges kommer den härledas från hello `<collection_expression>` med hjälp av följande regler:  
  
-   Om hello-uttrycket är ett samlingsnamn, kommer samlingsnamn användas som ett alias.  
  
-   Om hello-uttrycket är `<collection_expression>`, och sedan property_name sedan property_name kommer att användas som ett alias. Om hello-uttrycket är ett samlingsnamn, kommer samlingsnamn användas som ett alias.  
  
SOM`input_alias`  
  
Anger att hello `input_alias` är en uppsättning värden som returneras av hello underliggande samling uttryck.  
 
`input_alias`I  
  
Anger att hello `input_alias` bör vara hello uppsättning värden som hämtas av iterera över alla matriselement för varje matrisen som returneras av hello underliggande samling uttryck. Alla värden som returneras av underliggande samling uttryck som inte är en objektmatris ignoreras.  
  
`<collection_expression>`  
  
Anger hello samling uttryck toobe tooretrieve hello dokument.  
  
`ROOT`  
  
Anger att dokumentet ska hämtas från hello standard anslutna samling.  
  
`collection_name`  
  
Anger att dokumentet ska hämtas från hello tillhandahålls samling. hello namnet hello mängden måste matcha hello namnet på hello samlingen som är ansluten till.  
  
`input_alias`  
  
Anger att dokumentet ska hämtas från hello annan källa som definieras av hello som alias.  
  
`<collection_expression> '.' property_`  
  
Anger att dokumentet ska hämtas genom att öppna hello `property_name` egenskap eller array_index matriselement för alla dokument som hämtas av angivna samlingsuttrycket.  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
Anger att dokumentet ska hämtas genom att öppna hello `property_name` egenskap eller array_index matriselement för alla dokument som hämtas av angivna samlingsuttrycket.  
  
**Kommentarer**  
  
Alla alias tillhandahålls eller härleda i hello `<from_source>(`s) måste vara unika. hello Syntax `<collection_expression>.`property_name är hello samma som `<collection_expression>' ['"property_name"']'`. Hello senare syntaxen kan dock användas om ett egenskapsnamn som innehåller en icke-ID-tecken.  
  
**Saknar egenskaper, saknas matriselement, Odefinierad värden hantering**  
  
Om ett uttryck för samlingen använder egenskaper eller array-element och att värdet inte finns kan ignoreras värdet och inte bearbetas ytterligare.  
  
**Omfattningen för samlingen uttryck kontext**  
  
En samling uttryck kan vara samling omfång eller dokumentet omfattar:  
  
-   Ett uttryck som är begränsad samling, om hello underliggande för hello samling uttryck är antingen rot eller `collection_name`. Dessa uttryck representerar en uppsättning dokument som hämtats från hello samling direkt och är inte beroende av hello bearbetning av andra uttryck för samlingen.  
  
-   Ett uttryck är dokumentet omfång, om hello underliggande för hello samling uttryck är `input_alias` tidigare lanserades hello frågan. Dessa uttryck representerar en uppsättning dokument som erhålls genom att utvärdera hello samlingsuttrycket i hello omfång för varje dokument som tillhör toohello set som är associerade med samlingen för hello-alias.  hello resultatmängden blir en union av mängder som erhålls genom att utvärdera hello samlingsuttrycket för varje hello dokument i hello underliggande uppsättningen.  
  
**Kopplingar**  
  
I hello aktuella versionen stöder Azure Cosmos DB inre kopplingar. Anslut till ytterligare funktioner är kommande.

Inre kopplingar i en fullständig kryssprodukten av hello resultatuppsättningar deltar i hello koppling. hello resultatet av en N-vägs-koppling är en tuppelmängd N-element, där varje värde i hello tuppel är associerad med hello-alias som deltar i hello koppling och kan nås av refererar till detta alias i andra-satser.  
  
hello utvärdering av hello koppling beror på hello kontexten omfattning hello deltar anger:  
  
-  En koppling mellan samlingsuppsättningen A och samling omfång ange B, resulterar i en kryssprodukten för alla element i anger A och b
  
-   En koppling mellan uppsättning A och dokumentet omfång B, som resulterar i en union av alla uppsättningar som erhålls genom att utvärdera dokument-omfattande uppsättning B för varje dokument från ange A.  
  
 I hello aktuella versionen kan stöds högst ett omfång samling uttryck av hello frågeprocessorn.  
  
**Exempel på kopplingar:**  
  
Nu ska vi titta på hello efter FROM-satsen:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Låt varje källa definiera `input_alias1, input_alias2, …, input_aliasN`. FROM-satsen returnerar en mängd med N-tupplar (tuppel med N värden). Varje tuppel har värden som genereras av alla samling alias iterera över sina respektive uppsättningar.  
  
*Gå med i exempel 1, med 2 källor:*  
  
- Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.  
  
- Låt `<from_source2>` vara dokumentet definitionsområde refererar till input_alias1 och representerar anger:  
  
    {1, 2} för`input_alias1 = A,`  
  
    {3} för`input_alias1 = B,`  
  
    {4, 5} för`input_alias1 = C,`  
  
- Hej FROM-satsen `<from_source1> JOIN <from_source2>` resulterar i följande tupplar hello:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
*Ansluta till exempel 2, med 3 källor:*  
  
- Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.  
  
- Låt `<from_source2>` vara omfång dokumentet refererar till `input_alias1` och representerar anger:  
  
    {1, 2} för`input_alias1 = A,`  
  
    {3} för`input_alias1 = B,`  
  
    {4, 5} för`input_alias1 = C,`  
  
- Låt `<from_source3>` vara omfång dokumentet refererar till `input_alias2` och representerar anger:  
  
    {100, 200} för`input_alias2 = 1,`  
  
    {300} för`input_alias2 = 3,`  
  
- Hej FROM-satsen `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulterar i följande tupplar hello:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    A-, 1, 100 (A, 1, 200), (B, 3, 300)  
  
> [!NOTE]
> Avsaknad av tupplar för andra `input_alias1`, `input_alias2`, för vilka hello `<from_source3>` returnerade inte några värden.  
  
*Ansluta till exempel 3, med 3 källor:*  
  
- Låt < from_source1 > vara begränsad samling som representerar uppsättningen {A, B, C}.  
  
- Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.  
  
- Låt < from_source2 > att dokumentet omfång refererande input_alias1 och representerar anger:  
  
    {1, 2} för`input_alias1 = A,`  
  
    {3} för`input_alias1 = B,`  
  
    {4, 5} för`input_alias1 = C,`  
  
- Låt `<from_source3>` begränsas för`input_alias1` och representerar anger:  
  
    {100, 200} för`input_alias2 = A,`  
  
    {300} för`input_alias2 = C,`  
  
- Hej FROM-satsen `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulterar i följande tupplar hello:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    A-, 1, 100 (A, 1, 200), A-, 2, 100 A-, 2, 200 C, 4, 300, (C, 5, 300)  
  
> [!NOTE]
> Detta resulterade i kryssprodukten mellan `<from_source2>` och `<from_source3>` eftersom båda är begränsade toohello samma `<from_source1>`.  Detta resulterade i 4.2 2 x tupplar med värdet A, 0 tupplar med värdet B (1 x 0) och 2 (2 x 1) tupplar med värdet C.  
  
**Se även**  
  
 [SELECT-satsen](#bk_select_query)  
  
##  <a name="bk_where_clause"></a>WHERE-satsen  
 Anger hello sökvillkor för hello dokument som returneras av hello frågan.  
  
 **Syntax**  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 **Argument**  
  
-   `<filter_condition>`  
  
     Anger hello villkoret toobe för hello dokument toobe returneras.  
  
-   `<scalar_expression>`  
  
     Uttryck som representerar hello värdet toobe beräknas. Se hello [skaläruttryck](#bk_scalar_expressions) information.  
  
 **Kommentarer**  
  
 För att hello returnerade dokumentet toobe ett uttryck som angetts som filtervillkor måste utvärderas tootrue. Endast booleska värdet true ska uppfylla hello villkor och ett annat värde: Odefinierad, null, false, antalet, matris eller ett objekt ska inte uppfyller hello villkor.  
  
##  <a name="bk_orderby_clause"></a>ORDER BY-sats  
 Anger hello sorteringsordning för resultaten som returnerades av hello frågan.  
  
 **Syntax**  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 **Argument**  
  
-   `<sort_specification>`  
  
     Anger en egenskap eller ett uttryck som toosort hello frågan resultatuppsättningen. En sorteringskolumn kan anges som ett alias eller en kolumn.  
  
     Du kan ange flera sorteringskolumner. Kolumnnamnen måste vara unika. hello sifferföljd hello sorteringskolumner i hello ORDER BY-sats definierar hello organisation av hello sorteras resultatuppsättning. Att hello resultatet sorteras efter första hello-egenskapen och sedan den beställda listan sorteras efter andra hello-egenskapen och så vidare.  
  
     hello kolumnnamn som refereras i hello ORDER BY-satsen måste matcha tooeither en kolumn i hello Markera lista eller tooa kolumn som definierats i en tabell som har angetts i hello FROM-sats utan någon tvetydigheter.  
  
-   `<sort_expression>`  
  
     Anger en enskild egenskap eller ett uttryck på vilka toosort hello frågeresultatet.  
  
-   `<scalar_expression>`  
  
     Se hello [skaläruttryck](#bk_scalar_expressions) information.  
  
-   `ASC | DESC`  
  
     Anger att hello värden i hello angiven kolumn ska sorteras i stigande eller fallande ordning. ASC sorterar från hello lägsta värde toohighest värde. DESC sorterar från högsta värde toolowest värde. ASC är hello standardsorteringsordning. Null-värden behandlas som hello lägsta möjliga värden.  
  
 **Kommentarer**  
  
 Medan hello frågegrammatik stöder flera ordning efter egenskaper, stöder hello Azure Cosmos DB frågan runtime sortering bara mot en enskild egenskap och bara mot egenskapsnamn, d.v.s. inte mot beräknade egenskaper. Sortering kräver också att hello indexering principen innehåller ett intervall index för hello egenskap och hello anges typ med hello högsta precision. Läs toohello indexprincipdokumentationen för mer information.  
  
##  <a name="bk_scalar_expressions"></a>Skalära uttryck  
 Ett skalärt uttryck som är en kombination av symboler operatorer som kan utvärderas tooobtain ett enskilt värde. Enkla uttryck kan vara konstanter, egenskapsreferenser, matris referenser, alias referenser eller funktionsanrop. Enkla uttryck kan kombineras till komplexa uttryck med hjälp av operatörer.  
  
 Mer information om vilka skalärt uttryck som kan ha värden finns [konstanter](#bk_constants) avsnitt.  
  
 **Syntax**  
  
```  
<scalar_expression> ::=  
       <constant>   
     | input_alias   
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>   
     | <create_array_expression>  
     | (<scalar_expression>)   
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```  
  
 **Argument**  
  
-   `<constant>`  
  
     Representerar ett konstant värde. Se [konstanter](#bk_constants) information.  
  
-   `input_alias`  
  
     Representerar ett värde som definierats av hello `input_alias` introducerades i hello `FROM` satsen.  
    Det här värdet är garanterat toonot vara **Odefinierad** –**Odefinierad** värden i hello indata hoppas över.  
  
-   `<scalar_expression>.property_name`  
  
     Representerar ett värde för hello-egenskapen för ett objekt. Om hello egenskapen finns inte eller egenskapen refererar till ett värde som inte är ett objekt och sedan hello uttrycket utvärderas för**Odefinierad** värde.  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     Representerar ett värde för hello egenskap med namnet `property_name` eller array-elementet med index `array_index` av en objektmatris. Om hello egenskapen/matrisindex finns inte eller hello egenskapen/matrisindex refererar till ett värde som inte är en objektmatris, utvärderar hello uttryck tooundefined värde.  
  
-   `unary_operator <scalar_expression>`  
  
     Representerar en operator som används tooa enstaka värde. Se [operatörer](#bk_operators) information.  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     Representerar en operator som är kopplade tootwo värden. Se [operatörer](#bk_operators) information.  
  
-   `<scalar_function_expression>`  
  
     Representerar ett värde som definieras av ett resultat av ett funktionsanrop.  
  
-   `udf_scalar_function`  
  
     Namnet på hello användare definierats skalärfunktion.  
  
-   `builtin_scalar_function`  
  
     Namnet på hello inbyggda skalärfunktion.  
  
-   `<create_object_expression>`  
  
     Representerar ett värde som erhålls genom att skapa ett nytt objekt med angivna egenskaper och deras värden.  
  
-   `<create_array_expression>`  
  
     Representerar ett värde som erhålls genom att skapa en ny matris med angivna värden som element  
  
-   `parameter_name`  
  
     Representerar ett värde för hello angivna parameternamnet. Parameternamn måste ha en enda @ som hello första tecken.  
  
 **Kommentarer**  
  
 När du anropar en inbyggd eller användaren definierat skalärfunktion måste alla argument anges. Om någon av hello argument är odefinierad hello funktionen kommer inte att avbrytas och hello resultatet blir odefinierad.  
  
 När du skapar ett objekt, en egenskap som är tilldelad odefinierat värde hoppas över och inte ingår i hello-objekt.  
  
 När du skapar en matris, ett elementvärde som är tilldelad **Odefinierad** värdet kommer att hoppas över och inte ingår i hello skapade objektet. Detta innebär att hello nästa definierade element tootake dess plats så att hello skapade matris kommer inte ha hoppas över index.  
  
##  <a name="bk_operators"></a>Operatörer  
 Det här avsnittet beskrivs hello stöds operatörer. Varje operatör kan vara tilldelade tooexactly en kategori.  
  
 Se **operatorn kategorier** tabellen nedan, mer information om hantering av **Odefinierad** värden, krav för indatavärden för och hanteringen av värden med typer som inte matchar.  
  
 **Operatorn kategorier:**  
  
|**Kategori**|**Detaljer**|  
|-|-|  
|**aritmetiska**|Operatorn förväntar input(s) toobe nummer. Utdata är också ett tal. Om någon av hello indata är **Odefinierad** eller annan typ än antalet sedan hello resultatet är **Odefinierad**.|  
|**binär**|Operatorn förväntar input(s) toobe 32-bitars heltal nummer. Utdata är också 32-bitars heltal nummer.<br /><br /> Att kommer avrundas valfritt heltal. Ett positivt värde avrundas nedåt, negativa värden avrunda uppåt.<br /><br /> Ett värde som är utanför intervallet för 32-bitars heltal av hello konverteras med sista 32-bitar för dess två visas.<br /><br /> Om någon av hello indata är **Odefinierad** eller annan typ än siffra, och sedan hello resultatet är **Odefinierad**.<br /><br /> **Obs:** hello ovan beteendet är kompatibel med JavaScript binär operator-beteende.|  
|**logiska**|Operatorn förväntar input(s) toobe Boolean(s). Utdata är också ett booleskt värde.<br />Om någon av hello indata är **Odefinierad** eller annan typ än Boolean, och sedan hello resultatet **Odefinierad**.|  
|**jämförelse**|Operatorn förväntar input(s) toohave hello samma och inte är odefinierad. Utdata är ett booleskt värde.<br /><br /> Om någon av hello indata är **Odefinierad** eller hello indata har olika typer och sedan hello resultatet är **Odefinierad**.<br /><br /> Se **ordning med värden för jämförelse** tabellen för värdet ordning information.|  
|**sträng**|Operatorn förväntar input(s) toobe strängarna. Utdata är också en sträng.<br />Om någon av hello indata är **Odefinierad** eller annan typ än sträng sedan hello resultatet är **Odefinierad**.|  
  
 **Unära operatorer:**  
  
|**Namn**|**Operatorn**|**Detaljer**|  
|-|-|-|  
|**aritmetiska**|+<br /><br /> -|Returnerar hello numeriskt värde.<br /><br /> Binär negation. Returnerar negated numeriskt värde.|  
|**binär**|~|De som har komplementet. Returnerar en uppsättning ett numeriskt värde.|  
|**Logiska**|**INTE**|Negation. Returnerar negated booleskt värde.|  
  
 **Binära operatorer:**  
  
|**Namn**|**Operatorn**|**Detaljer**|  
|-|-|-|  
|**aritmetiska**|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|Tillägg.<br /><br /> Subtraktion.<br /><br /> Multiplikation.<br /><br /> Division.<br /><br /> Modulering.|  
|**binär**|&#124;<br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|Logiskt eller.<br /><br /> Binär och.<br /><br /> Bitvis XOR.<br /><br /> Vänsterskift.<br /><br /> Högerskift.<br /><br /> Noll fill högerskift.|  
|**logiska**|**OCH**<br /><br /> **ELLER**|Logisk konjunktion. Returnerar **SANT** om båda argument är **SANT**, returnerar **FALSKT** annars.<br /><br /> Logisk konjunktion. Returnerar **SANT** om båda argument är **SANT**, returnerar **FALSKT** annars.|  
|**jämförelse**|**=**<br /><br /> **!=, <>**<br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> **??**|Lika med. Returnerar **SANT** om argument är lika returnerar **FALSKT** annars.<br /><br /> Är inte lika med. Returnerar **SANT** om argumenten inte är lika, returnerar **FALSKT** annars.<br /><br /> Större än. Returnerar **SANT** om det första argumentet är större än hello andra returnerar **FALSKT** annars.<br /><br /> Större än eller lika med. Returnerar **SANT** om det första argumentet är större än eller lika med toohello andra, returnerar **FALSKT** annars.<br /><br /> Mindre än. Returnerar **SANT** om det första argumentet är mindre än hello andra, returnerar **FALSKT** annars.<br /><br /> Mindre än eller lika med. Returnerar **SANT** om det första argumentet är mindre än eller lika med toohello andra en returtyp **FALSKT** annars.<br /><br /> Slå samman. Returnerar hello andra argumentet om hello första argumentet är en **Odefinierad** värde.|  
|**Sträng**|**&#124;&#124;**|Sammanfogning. Returnerar en sammansättning av båda argumenten.|  
  
 **Ternär operator:**  
  
|Ternär operator|?|Returnerar hello andra argumentet om hello första argument evalueras för**SANT**; annars returnerar hello tredje argumentet.|  
|-|-|-|  
  
 **Sorteringen av värden för jämförelse**  
  
|**Typ**|**Värden ordning**|  
|-|-|  
|**Odefinierad**|Inte jämförbar.|  
|**Null**|Enstaka värde: **null**|  
|**Antal**|Naturliga tal.<br /><br /> Negativt oändligt värde är mindre än numeriskt värde.<br /><br /> Positivt oändligt värde är större än värdet värde. **NaN** värde är inte jämförbar. Jämföra med **NaN** leder **Odefinierad** värde.|  
|**Sträng**|Lexicographical ordning.|  
|**Matris**|Ingen ordning, men balanserad.|  
|**Objektet**|Ingen ordning, men balanserad.|  
  
 **Kommentarer**  
  
 I Azure Cosmos DB kallas ofta inte hello typer av värden tills de faktiskt har hämtats från hello-databasen. De flesta av hello operatörer har i ordning toosupport effektivt utförande av frågor, strikt krav. Operatörer ensamt utför även inte implicita konverteringar.  
  
 Det innebär att en fråga som: Välj * från ROOT r var r.Age = 21 returneras endast dokument med egenskapen ålder lika toohello nummer 21. Dokument med egenskapen ålder lika toohello strängen ”21” eller hello strängen ”0021” kommer inte matchar som hello uttryck ”21” = 21 utvärderar tooundefined. Detta ger en bättre användning av index, eftersom hello sökning efter ett visst värde (d.v.s. number 21) är snabbare än att söka efter ett obestämt antal möjliga matchningar (dvs. antalet 21 eller strängar ”21”, ”021”, ”21.0”...). Detta skiljer sig från hur JavaScript utvärderar operatorer på värden av olika typer.  
  
 **Jämförelse av och likhetsfrågor för matriser och -objekt**  
  
 Jämförelse av värden för matris eller ett objekt som använder range-operatorer (>, > =, <, < =) leder till odefinierad eftersom det inte finns inte ordning som har definierats för objektet eller matrisen värden. Men använder likhet/olikhet operatorer (=,! = <>) stöds och värden som ska jämföras strukturellt.  
  
 Matriser är lika om båda matriser har samma antal element och elementen på matchar positioner också är lika. Om att jämföra ett par element resulterar i odefinierat, hello resultatet av matrisen jämförelse är odefinierad.  
  
 Objekt är lika om både objekt har samma egenskaper som definierats och värden av matchande egenskaper också är lika. Om att jämföra ett par egenskapsvärden resulterar i odefinierat, hello resultatet av objektet jämförelse är odefinierad.  
  
##  <a name="bk_constants"></a>Konstanter  
 En konstant, även kallat ett litteralvärde eller ett skalärt värde är en symbol som representerar ett specifikt värde. hello-format för en konstant beror på hello datatypen för hello-värdet som representerar.  
  
 **Skalära datatyper som stöds:**  
  
|**Typ**|**Värden ordning**|  
|-|-|  
|**Odefinierad**|Enstaka värde: **Odefinierad**|  
|**Null**|Enstaka värde: **null**|  
|**Booleskt värde**|Värden: **FALSKT**, **SANT**.|  
|**Antal**|En dubbel precision flyttal, IEEE-754 som standard.|  
|**Sträng**|En sekvens med noll eller flera Unicode-tecken. Strängar måste stå inom enkla eller dubbla citattecken.|  
|**Matris**|En sekvens med noll eller flera element. Varje element kan vara ett värde med en skalär datatypen, utom Undefined.|  
|**Objektet**|En osorterad uppsättning noll eller flera namn/värde-par. Namnet är en Unicode-sträng, värdet kan vara av valfri skalära datatyp, utom **Undefined**.|  
  
 **Syntax**  
  
```  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
 **Argument**  
  
1.  `<undefined_constant>; undefined`  
  
     Representerar odefinierat värde av typen odefinierad.  
  
2.  `<null_constant>; null`  
  
     Representerar **null** värde av typen **Null**.  
  
3.  `<boolean_constant>`  
  
     Representerar konstanter av typen Boolean.  
  
4.  `false`  
  
     Representerar **FALSKT** värde av typen Boolean.  
  
5.  `true`  
  
     Representerar **SANT** värde av typen Boolean.  
  
6.  `<number_constant>`  
  
     Representerar en konstant.  
  
7.  `decimal_literal`  
  
     Decimal literaler är nummer som representeras med decimalform eller matematisk notation.  
  
8.  `hexadecimal_literal`  
  
     Hexadecimala strängar är nummer som representeras med prefixet '0 x' följt av en eller flera hexadecimala siffror.  
  
9. `<string_constant>`  
  
     Representerar en konstant av typen String.  
  
10. `string _literal`  
  
     Stränglitteraler är Unicode-strängar som representeras av en följd av noll eller flera Unicode-tecken eller escape-sekvenser. Stränglitteraler omges av enkla citattecken (apostrof: ') eller dubbla citattecken (citattecken ”:).  
  
 Följande escape-sekvenser tillåts:  
  
|**Escape-sekvens**|**Beskrivning**|**Unicode-tecken**|  
|-|-|-|  
|\\'|apostrof (')|U + 0027|  
|\\"|citattecken (”)|U + 0022|  
|\\\|omvänd solidus (\\)|U + 005C|  
|\\/|solidus (/)|U + 002F|  
|\b|BACKSTEG|U + 0008|  
|\f|formuläret feed|U + 000C|  
|\n|radmatning|U + 000A|  
|\r|vagnretur|U + 000D|  
|\t|fliken|U + 0009|  
|\uXXXX|En Unicode-tecken som definieras av 4 hexadecimala siffror.|U + XXXX|  
  
##  <a name="bk_query_perf_guidelines"></a>Riktlinjer för frågan prestanda  
 Det bör använda filter som kan hanteras via ett eller flera index för en fråga toobe körs effektivt för en stor samling.  
  
 följande filter hello beaktas för index sökning:  
  
-   Använd Likhetsoperatorn (=) med ett sökvägsuttryck för dokumentet och en konstant.  
  
-   Använd range-operatorer (<, \<=, >, > =) med ett sökvägsuttryck för dokumentet och antalet konstanter.  
  
-   Dokumentet sökvägsuttryck står för ett uttryck som identifierar en konstant sökväg i hello dokument från hello refererar till databasen samling.  
  
 **Dokumentet sökvägsuttryck**  
  
 Dokumentet sökvägsuttryck är uttryck som en sökväg till matrisen egenskapen eller indexeraren bedömare över ett dokument som kommer från databasen samling dokument. Den här sökvägen kan vara används tooidentify hello platsen för värden som refereras i ett filter direkt i hello dokument i hello databasen samling.  
  
 För ett uttryck toobe anses vara ett dokument sökvägsuttryck, bör det:  
  
1.  Referens hello samling root direkt.  
  
2.  Indexerare för referens-egenskapen eller konstant matris för vissa dokument sökvägsuttryck  
  
3.  Referera till ett alias som representerar vissa dokument sökvägsuttryck.  
  
     **Syntaxen konventioner**  
  
     hello i den följande tabellen beskrivs hello konventioner toodescribe syntax i hello frågespråket i DocumentDB API-referens.  
  
    |**Konventionen**|**Används för**|  
    |-|-|    
    |VERSALER|Skiftlägeskänsliga nyckelord.|  
    |gemener|Skiftlägeskänsligt nyckelord.|  
    |\<nonterminal >|Öppen, som har definierats separat.|  
    |\<nonterminal >:: =|Syntax för definition av hello nonterminal.|  
    |other_terminal|Terminal (token), som beskrivs i detalj i ord.|  
    |Identifierare|Identifierare. Gör följande endast tecken: a-z A-Z 0-9 _First tecknet får inte vara en siffra.|  
    |”sträng”|Sträng inom citattecken. Gör att en giltig sträng. Se beskrivning av en string_literal.|  
    |'symbolen'|Literalen symbol som ingår i hello syntax.|  
    |&#124; (lodrätt streck)|Alternativ för syntax objekt. Du kan använda endast en av de angivna hello-objekt.|  
    |[] /(brackets)|Hakparenteser innefatta en eller flera valfria objekt.|  
    |[,.. .n]|Anger hello föregående artikel kan vara upprepade n är antalet gånger. hello förekomster avgränsas med kommatecken.|  
    |[.. .n]|Anger hello föregående artikel kan vara upprepade n är antalet gånger. hello förekomster avgränsas med blanksteg.|  
  
##  <a name="bk_built_in_functions"></a>Inbyggda funktioner  
 Azure Cosmos-DB innehåller många inbyggda SQL-funktioner. hello kategorier av inbyggda funktioner i listan nedan.  
  
|Funktionen|Beskrivning|  
|--------------|-----------------|  
|[Matematiska funktioner](#bk_mathematical_functions)|hello matematiska funktioner varje utför en beräkning, vanligtvis baserat på värden som har angetts som argument och returnerar ett numeriskt värde.|  
|[Ange kontrollerar funktioner](#bk_type_checking_functions)|hello typ kontrollerar funktioner kan du toocheck hello typ av ett uttryck i SQL-frågor.|  
|[Strängfunktioner](#bk_string_functions)|hello strängfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde.|  
|[Matrisfunktioner](#bk_array_functions)|hello matrisfunktioner kan du utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde.|  
|[Spatial funktioner](#bk_spatial_functions)|hello spatial funktioner utföra en åtgärd på ett indatavärde Rumsobjektet och returnera ett numeriskt eller booleskt värde.|  
  
###  <a name="bk_mathematical_functions"></a>Matematiska funktioner  
 hello utför följande funktioner en beräkning, vanligtvis baserat på värden som har angetts som argument och returnerar ett numeriskt värde.  
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ARCCOS](#bk_acos)|[ARCSIN](#bk_asin)|  
|[ARCTAN](#bk_atan)|[ATN2](#bk_atn2)|[TAK](#bk_ceiling)|  
|[COS](#bk_cos)|[BET](#bk_cot)|[GRADER](#bk_degrees)|  
|[EXP](#bk_exp)|[VÅNING](#bk_floor)|[LOGG](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[POWER](#bk_power)|  
|[RADIANER](#bk_radians)|[AVRUNDA](#bk_round)|[SIN](#bk_sin)|  
|[ROT](#bk_sqrt)|[RUTA](#bk_square)|[LOGGA](#bk_sign)|  
|[TAN](#bk_tan)|[AVKORTA](#bk_trunc)||  
  
####  <a name="bk_abs"></a>ABS  
 Returnerar hello absolut (positivt) värdet för hello angetts numeriskt uttryck.  
  
 **Syntax**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello visar följande exempel hello resultatet av funktionen hello ABS på tre olika nummer.  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <a name="bk_acos"></a>ARCCOS  
 Returnerar hello vinkel angivna i radianer, vars cosinus är hello numeriska uttrycket; kallas även cosinus.  
  
 **Syntax**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returneras följande exempel hello ARCCOS 1.  
  
```  
SELECT ACOS(-1)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a>ARCSIN  
 Returnerar hello vinkel angivna i radianer, vars sinus är hello numeriska uttrycket. Detta kallas också arcsinus.  
  
 **Syntax**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returneras följande exempel hello ARCSIN 1.  
  
```  
SELECT ASIN(-1)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a>ARCTAN  
 Returnerar hello vinkel angivna i radianer, vars tangens är hello numeriska uttrycket. Detta kallas också tangens.  
  
 **Syntax**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 följande exempel returnerar hello ARCTAN av hello hello angivet värde.  
  
```  
SELECT ATAN(-45.01)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a>ATN2  
 Returnerar hello huvudvärdet hello arcus tangens för y / x, uttryckt i radianer.  
  
 **Syntax**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello följande exempel beräknas hello ATN2 för hello angetts x och y komponenter.  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a>TAK  
 Returnerar hello minsta heltal som är större än eller lika med för att hello uttryck.  
  
 **Syntax**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello som följande exempel visar positivt numeriskt negativt, och noll värden med hello TAKET funktion.  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <a name="bk_cos"></a>COS  
 Returnerar hello trigonometriska värden cosinus för hello angiven vinkel i radianer, i hello angivna uttrycket.  
  
 **Syntax**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 följande exempel hello beräknar hello COS på hello vinkeln som anges.  
  
```  
SELECT COS(14.78)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a>BET  
 Returnerar hello trigonometriska värden cotangens för hello angiven vinkel i radianer, i hello anges numeriskt uttryck.  
  
 **Syntax**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 följande exempel hello beräknar hello bet för hello angivna vinkeln.  
  
```  
SELECT COT(124.1332)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a>GRADER  
 Returnerar hello motsvarande vinkel i grader för en vinkel angiven i radianer.  
  
 **Syntax**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returnerar följande exempel hello antal grader i vinkeln i radianer PI/2.  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 90}]  
```  
  
####  <a name="bk_floor"></a>VÅNING  
 Returnerar hello största heltal som är mindre än eller lika toohello angivna numeriska uttrycket.  
  
 **Syntax**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello som följande exempel visar positivt numeriskt negativt, och noll värden med hello VÅNING funktion.  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <a name="bk_exp"></a>EXP  
 Returnerar hello exponentiell värdet för hello angetts numeriskt uttryck.  
  
 **Syntax**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Kommentarer**  
  
 hello konstant **e** (2.718281...) är hello basen för den naturliga logaritmen.  
  
 hello e upphöjt till ett tal är hello konstant **e** aktiveras toohello power hello tal. Till exempel EXP(1.0) = e ^ 1.0 = 2.71828182845905 och EXP(10) = e ^ 10 = 22026.4657948067.  
  
 hello e upphöjt till hello naturliga logaritmen för ett tal är hello tal sig själv: EXP (loggning (n)) = n. Och hello naturliga logaritmen för hello e upphöjt till ett tal är hello sig själv: LOGG (EXP (n)) = n.  
  
 **Exempel**  
  
 hello följande exempel deklarerar en variabel och returnerar hello exponentiell hello specifik variabel (10).  
  
```  
SELECT EXP(10)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 hello returnerar följande exempel hello exponentiell värdet hello naturliga logaritmen för 20 och hello naturliga logaritmen för hello e upphöjt till 20. Eftersom dessa funktioner är inverterade funktioner för varandra, hello returvärdet med avrundning för beräkningar i båda fallen är 20 flyttal.  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <a name="bk_log"></a>LOGG  
 Returnerar hello naturliga logaritmen för hello angivna numeriska uttrycket.  
  
 **Syntax**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
-   `base`  
  
     Valfritt numeriskt argument som anger hello för hello logaritmen.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Kommentarer**  
  
 Som standard returnerar LOG() hello naturliga logaritmen. Du kan ändra hello basen för hello logaritmen tooanother värde med hjälp av valfri grundläggande hello-parameter.  
  
 hello naturliga logaritmen är hello logaritmen toohello grundläggande **e**, där **e** är en onormal konstant ungefär samma too2.718281828.  
  
 hello naturliga logaritmen för hello e upphöjt till ett tal är hello tal sig själv: LOGG (EXP (n)) = n. Och hello e upphöjt till hello naturliga logaritmen för ett tal är hello sig själv: EXP (loggning (n)) = n.  
  
 **Exempel**  
  
 hello följande exempel deklarerar en variabel och returnerar hello logaritmen hello specifik variabel (10).  
  
```  
SELECT LOG(10)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 hello beräknar följande exempel hello LOGGEN för hello e upphöjt till ett tal.  
  
```  
SELECT EXP(LOG(10))  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a>LOG10  
 Returnerar hello 10-logaritmen av hello angivna numeriska uttrycket.  
  
 **Syntax**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Kommentarer**  
  
 hello LOG10 och POWER funktionerna är omvänt relaterade tooone en annan. Till exempel 10 ^ LOG10(n) = n.  
  
 **Exempel**  
  
 hello följande exempel deklarerar en variabel och returnerar hello LOG10 hello specifik variabel (100).  
  
```  
SELECT LOG10(100)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 2}]  
```  
  
####  <a name="bk_pi"></a>PI  
 Returnerar hello konstantvärde pi.  
  
 **Syntax**  
  
```  
PI ()  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returnerar följande exempel värdet PI hello.  
  
```  
SELECT PI()  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a>POWER  
 Returnerar hello hello anges värdet uttryck toohello angetts power.  
  
 **Syntax**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
-   `y`  
  
     Är hello power toowhich tooraise `numeric_expression`.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello som följande exempel visar höja toohello tal upphöjt till 3 (nummer hello hello kub).  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <a name="bk_radians"></a>RADIANER  
 Returnerar radianer när ett numeriskt uttryck i grader anges.  
  
 **Syntax**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello följande exempel tar några vinklar som indata och returnerar motsvarande radian värden.  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a>AVRUNDA  
 Returnerar ett numeriskt värde avrundade toohello närmaste heltal.  
  
 **Syntax**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello Avrundar följande exempel hello efter positiva och negativa tal toohello närmsta heltal.  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <a name="bk_sign"></a>LOGGA  
 Returnerar hello positiv (+ 1), noll (0) eller minustecken (-1) för hello angetts numeriskt uttryck.  
  
 **Syntax**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returnerar följande exempel hello LOGGA värdena av talen från too2-2.  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <a name="bk_sin"></a>SIN  
 Returnerar hello trigonometriska värden sinus för hello angiven vinkel i radianer, i hello angivna uttrycket.  
  
 **Syntax**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 följande exempel hello beräknar hello SIN för hello angivna vinkeln.  
  
```  
SELECT SIN(45.175643)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a>ROT  
 Returnerar hello kvadratroten ur hello angetts numeriskt värde.  
  
 **Syntax**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returneras följande exempel hello kvadraten rötter av talen 1-3.  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a>RUTA  
 Returnerar hello rektangulär hello angetts numeriskt värde.  
  
 **Syntax**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returneras följande exempel hello kvadraten av talen 1-3.  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <a name="bk_tan"></a>TAN  
 Returnerar hello tangens för hello angiven vinkel i radianer, i hello angivna uttrycket.  
  
 **Syntax**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello följande exempel beräknas hello tangens för PI () / 2.  
  
```  
SELECT TAN(PI()/2);  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a>AVKORTA  
 Returnerar ett numeriskt värde, trunkerat toohello närmaste heltal.  
  
 **Syntax**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argument**  
  
-   `numeric_expression`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 följande exempel hello trunkerar hello efter positiva och negativa tal toohello närmsta heltal.  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <a name="bk_type_checking_functions"></a>Ange kontrollerar funktioner  
 hello följande funktioner stöder typkontroll mot indatavärden och varje returnera ett booleskt värde.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a>IS_ARRAY  
 Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är en matris.  
  
 **Syntax**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_ARRAY funktion.  
  
```  
SELECT   
 IS_ARRAY(true),   
 IS_ARRAY(1),  
 IS_ARRAY("value"),  
 IS_ARRAY(null),  
 IS_ARRAY({prop: "value"}),   
 IS_ARRAY([1, 2, 3]),  
 IS_ARRAY({prop: "value"}.prop2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <a name="bk_is_bool"></a>IS_BOOL  
 Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett booleskt värde.  
  
 **Syntax**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_BOOL funktion.  
  
```  
SELECT   
    IS_BOOL(true),   
    IS_BOOL(1),  
    IS_BOOL("value"),   
    IS_BOOL(null),  
    IS_BOOL({prop: "value"}),   
    IS_BOOL([1, 2, 3]),  
    IS_BOOL({prop: "value"}.prop2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_defined"></a>IS_DEFINED  
 Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde.  
  
 **Syntax**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 följande exempel söker efter hello förekomsten av en egenskap i hello hello angetts JSON-dokumentet. hello returnerar först SANT eftersom ”a” finns, men hello andra returnerar värdet false eftersom ”b” saknas.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <a name="bk_is_null"></a>IS_NULL  
 Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är null.  
  
 **Syntax**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_NULL funktion.  
  
```  
SELECT   
    IS_NULL(true),   
    IS_NULL(1),  
    IS_NULL("value"),   
    IS_NULL(null),  
    IS_NULL({prop: "value"}),   
    IS_NULL([1, 2, 3]),  
    IS_NULL({prop: "value"}.prop2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_number"></a>IS_NUMBER  
 Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett tal.  
  
 **Syntax**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_NULL funktion.  
  
```  
SELECT   
    IS_NUMBER(true),   
    IS_NUMBER(1),  
    IS_NUMBER("value"),   
    IS_NUMBER(null),  
    IS_NUMBER({prop: "value"}),   
    IS_NUMBER([1, 2, 3]),  
    IS_NUMBER({prop: "value"}.prop2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <a name="bk_is_object"></a>IS_OBJECT  
 Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett JSON-objekt.  
  
 **Syntax**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_OBJECT funktion.  
  
```  
SELECT   
    IS_OBJECT(true),   
    IS_OBJECT(1),  
    IS_OBJECT("value"),   
    IS_OBJECT(null),  
    IS_OBJECT({prop: "value"}),   
    IS_OBJECT([1, 2, 3]),  
    IS_OBJECT({prop: "value"}.prop2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <a name="bk_is_primitive"></a>IS_PRIMITIVE  
 Returnerar ett booleskt värde som anger om hello hello angetts uttrycket är en primitiv (string, Boolean, numeriska eller null).  
  
 **Syntax**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_PRIMITIVE funktion.  
  
```  
SELECT   
           IS_PRIMITIVE(true),   
           IS_PRIMITIVE(1),  
           IS_PRIMITIVE("value"),   
           IS_PRIMITIVE(null),  
           IS_PRIMITIVE({prop: "value"}),   
           IS_PRIMITIVE([1, 2, 3]),  
           IS_PRIMITIVE({prop: "value"}.prop2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <a name="bk_is_string"></a>IS_STRING  
 Returnerar ett booleskt värde som anger om hello hello angetts uttryck är en sträng.  
  
 **Syntax**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argument**  
  
-   `expression`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_STRING funktion.  
  
```  
SELECT   
       IS_STRING(true),   
       IS_STRING(1),  
       IS_STRING("value"),   
       IS_STRING(null),  
       IS_STRING({prop: "value"}),   
       IS_STRING([1, 2, 3]),  
       IS_STRING({prop: "value"}.prop2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <a name="bk_string_functions"></a>Strängfunktioner  
 hello följande skalärfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde.  
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[INNEHÅLLER](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[VÄNSTER](#bk_left)|[LÄNGD](#bk_length)|  
|[LÄGRE](#bk_lower)|[LTRIM](#bk_ltrim)|[ERSÄTT](#bk_replace)|  
|[Replikera](#bk_replicate)|[OMVÄND](#bk_reverse)|[HÖGER](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[DELSTRÄNGEN](#bk_substring)|  
|[ÖVRE](#bk_upper)|||  
  
####  <a name="bk_concat"></a>CONCAT  
 Returnerar en sträng som är hello resultat sammanfoga två eller flera strängvärden.  
  
 **Syntax**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 följande exempel returnerar hello sammanfogas sträng med hello hello värden har angetts.  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <a name="bk_contains"></a>INNEHÅLLER  
 Returnerar ett booleskt värde som anger om hello första stränguttryck innehåller hello andra.  
  
 **Syntax**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello kontrollerar följande exempel om ”abc” innehåller ”ab” och ”d”.  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_endswith"></a>ENDSWITH  
 Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra.  
  
 **Syntax**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello returneras följande exempel hello ”abc” slutar med ”b” och ”bc”.  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_index_of"></a>INDEX_OF  
 Returnerar hello startposition för hello första förekomsten av hello andra stränguttryck inom hello första angivet stränguttryck eller -1 om hello strängen inte hittas.  
  
 **Syntax**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 hello returnerar följande exempel hello index med olika delsträngar inuti ”abc”.  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <a name="bk_left"></a>VÄNSTER  
 Returnerar hello vänstra del av en sträng med hello angivna antalet tecken.  
  
 **Syntax**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
-   `num_expr`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 hello följande exempel returneras hello kvar en del av ”abc” för olika värden för längd.  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <a name="bk_length"></a>LÄNGD  
 Returnerar hello antalet tecken i hello angetts stränguttryck.  
  
 **Syntax**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 hello returneras följande exempel hello stränglängden.  
  
```  
SELECT LENGTH("abc")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_lower"></a>LÄGRE  
 Returnerar ett stränguttryck efter konvertering versal data toolowercase.  
  
 **Syntax**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 följande exempel visar hur hello toouse längst ned i en fråga.  
  
```  
SELECT LOWER("Abc")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a>LTRIM  
 Returnerar ett stränguttryck när den tar bort inledande blanksteg.  
  
 **Syntax**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 följande exempel visar hur hello toouse LTRIM i en fråga.  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a>ERSÄTT  
 Ersätter alla förekomster av ett angivet strängvärde med ett annat värde.  
  
 **Syntax**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 hello som följande exempel visar hur toouse Ersätt i en fråga.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a>REPLIKERA  
 Upprepar ett strängvärde till ett angivet antal gånger.  
  
 **Syntax**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
-   `num_expr`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 hello som följande exempel visar hur toouse replikeras i en fråga.  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a>OMVÄND  
 Returnerar hello omvänd ordning i ett strängvärde.  
  
 **Syntax**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 hello som följande exempel visar hur toouse OMVÄND i en fråga.  
  
```  
SELECT REVERSE("Abc")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <a name="bk_right"></a>HÖGER  
 Returnerar hello just en del av en sträng med hello angivet antal tecken.  
  
 **Syntax**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
-   `num_expr`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 hello returneras följande exempel hello högra delen av ”abc” för olika värden för längd.  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a>RTRIM  
 Returnerar ett stränguttryck när den tar bort avslutande blanksteg.  
  
 **Syntax**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 följande exempel visar hur hello toouse RTRIM i en fråga.  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a>STARTSWITH  
 Returnerar ett booleskt värde som anger om hello första stränguttryck börjar med hello andra.  
  
 **Syntax**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 hello följande exempel kontrollerar om hello strängen ”abc” börjar med ”b” och ”a”.  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <a name="bk_substring"></a>DELSTRÄNGEN  
 Returnerar en del av ett stränguttryck som börjar vid hello angetts nollbaserade teckenposition och fortsätter toohello angiven längd eller toohello slutet av hello strängen.  
  
 **Syntax**  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
-   `num_expr`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 hello följande exempel returneras hello delsträngen ”ABC” vid 1 och en längd på 1 tecken.  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "b"}]  
```  
  
####  <a name="bk_upper"></a>ÖVRE  
 Returnerar ett stränguttryck efter konvertering gemen data toouppercase.  
  
 **Syntax**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argument**  
  
-   `str_expr`  
  
     Är ett stränguttryck.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck.  
  
 **Exempel**  
  
 följande exempel visar hur hello toouse längst upp i en fråga  
  
```  
SELECT UPPER("Abc")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <a name="bk_array_functions"></a>Matrisfunktioner  
 hello följande skalärfunktioner utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde  
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a>ARRAY_CONCAT  
 Returnerar en matris som är hello resultat sammanfoga två eller flera matrisvärden.  
  
 **Syntax**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argument**  
  
-   `arr_expr`  
  
     Är ett giltigt array-uttryck.  
  
 **Returtyper**  
  
 Returnerar en matrisuttryck.  
  
 **Exempel**  
  
 Hej följande exempel hur tooconcatenate två matriser.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a>ARRAY_CONTAINS  
 Returnerar ett booleskt värde som anger om hello matris innehåller hello anges värdet.  
  
 **Syntax**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 **Argument**  
  
-   `arr_expr`  
  
     Är ett giltigt array-uttryck.  
  
-   `expr`  
  
     Är ett giltigt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt värde.  
  
 **Exempel**  
  
 följande exempel på hur hello toocheck för medlemskap i en matris med ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <a name="bk_array_length"></a>ARRAY_LENGTH  
 Returnerar hello antalet element i hello angetts matrisuttryck.  
  
 **Syntax**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argument**  
  
-   `arr_expr`  
  
     Är ett giltigt array-uttryck.  
  
 **Returtyper**  
  
 Returnerar ett numeriskt uttryck.  
  
 **Exempel**  
  
 Hej följande exempel på hur tooget hello längden på en matris med ARRAY_LENGTH.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{"$1": 3}]  
```  
  
####  <a name="bk_array_slice"></a>ARRAY_SLICE  
 Returnerar en del av ett matrisuttryck.
  
 **Syntax**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argument**  
  
-   `arr_expr`  
  
     Är ett giltigt array-uttryck.  
  
-   `num_expr`  
  
     Är ett numeriskt uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt värde.  
  
 **Exempel**  
  
 följande exempel på hur hello tooget en del av en matris med ARRAY_SLICE.  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <a name="bk_spatial_functions"></a>Spatial funktioner  
 hello följande skalärfunktioner utföra en åtgärd på ett indatavärde Rumsobjektet och returnera ett numeriskt eller booleskt värde.  
  
||||  
|-|-|-|  
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|  
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)|||  
  
####  <a name="bk_st_distance"></a>ST_DISTANCE  
 Returnerar hello avståndet mellan hello två GeoJSON punkt, Polygon eller LineString uttryck.  
  
 **Syntax**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argument**  
  
-   `spatial_expr`  
  
     Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.  
  
 **Returtyper**  
  
 Returnerar ett stränguttryck som innehåller hello avstånd. Detta uttrycks i mätare för hello standard referenssystem.  
  
 **Exempel**  
  
 följande exempel visar hur hello tooreturn alla family dokument som ligger inom 30 km av hello anges platsen med hjälp av hello ST_DISTANCE inbyggd funktion. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a>ST_WITHIN  
 Returnerar ett booleskt uttryck som anger om hello GeoJSON (Point, LineString eller Polygon) det angivna objektet i hello första argumentet ligger inom hello GeoJSON (Point, LineString eller Polygon) i hello andra argumentet.  
  
 **Syntax**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argument**  
  
-   `spatial_expr`  
  
     Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.  
 
-   `spatial_expr`  
  
     Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.  
  
 **Returtyper**  
  
 Returnerar ett booleskt värde.  
  
 **Exempel**  
  
 hello som följande exempel visar hur toofind alla familj dokument inom en polygon med ST_WITHIN.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a>ST_INTERSECTS  
 Returnerar ett booleskt uttryck som anger om hello GeoJSON (Point, LineString eller Polygon) det angivna objektet i hello första argumentet korsar hello GeoJSON (Point, LineString eller Polygon) i hello andra argumentet.  
  
 **Syntax**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argument**  
  
-   `spatial_expr`  
  
     Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.  
 
-   `spatial_expr`  
  
     Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.  
  
 **Returtyper**  
  
 Returnerar ett booleskt värde.  
  
 **Exempel**  
  
 följande exempel visar hur hello toofind alla områden som hello angivna polygon.  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a>ST_ISVALID  
 Returnerar ett booleskt värde som anger om hello angiven GeoJSON punkt, Polygon eller LineString uttryck är giltig.  
  
 **Syntax**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argument**  
  
-   `spatial_expr`  
  
     Är ett giltigt GeoJSON punkt, Polygon eller LineString uttryck.  
  
 **Returtyper**  
  
 Returnerar ett booleskt uttryck.  
  
 **Exempel**  
  
 följande exempel visar hur hello toocheck om en är giltig med ST_VALID.  
  
 Den här platsen har till exempel en latitud-värde som inte är i hello giltiga värdeintervallet [-90, 90], så hello frågan returnerar FALSKT.  
  
 För polygoner hello GeoJSON specifikationen kräver hello senaste koordinaten par angivna att hello samma som hello först toocreate en sluten form. Punkter i en polygon måste anges i följd medurs ordning. En polygon som angetts i medurs ordning representerar hello inversen av hello region i den.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{ "$1": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED  
 Returnerar ett JSON-värde som innehåller ett booleskt värde om hello angivna GeoJSON punkt, Polygon eller LineString uttrycket är giltig och om det är ogiltig, dessutom hello orsak som ett strängvärde.  
  
 **Syntax**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argument**  
  
-   `spatial_expr`  
  
     Är ett giltigt GeoJSON punkt eller polygon uttryck.  
  
 **Returtyper**  
  
 Returnerar ett JSON-värde som innehåller ett booleskt värde om hello anges GeoJSON punkt eller polygon uttryck är giltigt och om det är ogiltig, dessutom hello orsak som ett strängvärde.  
  
 **Exempel**  
  
 följande exempel på hur hello toocheck giltig (med information) med hjälp av ST_ISVALIDDETAILED.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 Här är hello resultatuppsättning.  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a polygon must have hello same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a>Nästa steg  
 [SQL-syntax och SQL-fråga för Azure Cosmos DB](documentdb-sql-query.md)   
 [Azure DB Cosmos-dokumentation](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
