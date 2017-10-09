---
<span data-ttu-id="2bc11-101">Rubrik: aaa ”Azure Cosmos DB DocumentDB API: SQL-syntaxen | Microsoft Docs ”beskrivning: referera till dokumentationen för hello Azure Cosmos DB DocumentDB API SQL-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-101">title: aaa"Azure Cosmos DB DocumentDB API: SQL syntax | Microsoft Docs" description: Reference documentation for hello Azure Cosmos DB DocumentDB API SQL query language.</span></span>
<span data-ttu-id="2bc11-102">tjänster: cosmos-db författare: mimig1 manager: jhubbard editor: mimig dokumentationcenter: ''</span><span class="sxs-lookup"><span data-stu-id="2bc11-102">services: cosmos-db author: mimig1 manager: jhubbard editor: mimig documentationcenter: ''</span></span>

<span data-ttu-id="2bc11-103">MS.AssetID: ms.service: cosmos-db ms.workload: data services ms.tgt_pltfrm: na ms.devlang: na ms.topic: referera ms.date: 06/13/2017 ms.author: mimig</span><span class="sxs-lookup"><span data-stu-id="2bc11-103">ms.assetid: ms.service: cosmos-db ms.workload: data-services ms.tgt_pltfrm: na ms.devlang: na ms.topic: reference ms.date: 06/13/2017 ms.author: mimig</span></span>

---

# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a><span data-ttu-id="2bc11-104">Azure Cosmos DB DocumentDB API: Referens SQL-syntax</span><span class="sxs-lookup"><span data-stu-id="2bc11-104">Azure Cosmos DB DocumentDB API: SQL syntax reference</span></span>

<span data-ttu-id="2bc11-105">hello Azure DB-API Cosmos DocumentDB stöder förfrågningar till dokument med en bekant SQL (Structured Query Language) som grammatik via hierarkiska JSON-dokument utan explicita schema eller att sekundärindex.</span><span class="sxs-lookup"><span data-stu-id="2bc11-105">hello Azure Cosmos DB DocumentDB API supports querying documents using a familiar SQL (Structured Query Language) like grammar over hierarchical JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> <span data-ttu-id="2bc11-106">Det här avsnittet finns i referensdokumentationen för hello DocumentDB API SQL-frågespråket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-106">This topic provides reference documentation for hello DocumentDB API SQL query language.</span></span>

<span data-ttu-id="2bc11-107">En genomgång av hello DocumentDB API SQL-frågespråket finns [SQL-frågor för Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="2bc11-107">For a walkthrough of hello DocumentDB API SQL query language, see [SQL queries for Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).</span></span>  
  
<span data-ttu-id="2bc11-108">Vi också bjuda in dig toovisit hello [Query Playground](http://www.documentdb.com/sql/demo) där du kan prova Azure Cosmos DB och köra SQL-frågor mot vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-108">We also invite you toovisit hello [Query Playground](http://www.documentdb.com/sql/demo) where you can try Azure Cosmos DB and run SQL queries against our dataset.</span></span>  
  
## <a name="select-query"></a><span data-ttu-id="2bc11-109">SELECT-frågan</span><span class="sxs-lookup"><span data-stu-id="2bc11-109">SELECT query</span></span>  
<span data-ttu-id="2bc11-110">Hämtar JSON-dokument från hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-110">Retrieves JSON documents from hello database.</span></span> <span data-ttu-id="2bc11-111">Stöder uttrycksutvärdering, projektioner filtrering och ansluter till.</span><span class="sxs-lookup"><span data-stu-id="2bc11-111">Supports expression evaluation, projections, filtering and joins.</span></span>  <span data-ttu-id="2bc11-112">hello-konventioner som används för att beskriva hello SELECT-satser är en tabell i hello Syntax konventioner avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-112">hello conventions used for describing hello SELECT statements are tabulated in hello Syntax conventions section.</span></span>  
  
<span data-ttu-id="2bc11-113">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-113">**Syntax**</span></span>  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 <span data-ttu-id="2bc11-114">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-114">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-115">Se följande avsnitt för mer information om varje sats:</span><span class="sxs-lookup"><span data-stu-id="2bc11-115">See following sections for details on each clause:</span></span>  
  
-   [<span data-ttu-id="2bc11-116">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-116">SELECT clause</span></span>](#bk_select_query)  
  
-   [<span data-ttu-id="2bc11-117">FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-117">FROM clause</span></span>](#bk_from_clause)  
  
-   [<span data-ttu-id="2bc11-118">WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-118">WHERE clause</span></span>](#bk_where_clause)  
  
-   [<span data-ttu-id="2bc11-119">ORDER BY-sats</span><span class="sxs-lookup"><span data-stu-id="2bc11-119">ORDER BY clause</span></span>](#bk_orderby_clause)  
  
<span data-ttu-id="2bc11-120">hello-satser i hello SELECT-instruktion måste beställas som ovan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-120">hello clauses in hello SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="2bc11-121">En av valfri hello-satser kan utelämnas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-121">Any one of hello optional clauses can be omitted.</span></span> <span data-ttu-id="2bc11-122">Men när valfria satser används, måste de visas i hello rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-122">But when optional clauses are used, they must appear in hello right order.</span></span>  
  
<span data-ttu-id="2bc11-123">**Logiska behandlingsordning hello SELECT-instruktion**</span><span class="sxs-lookup"><span data-stu-id="2bc11-123">**Logical Processing Order of hello SELECT statement**</span></span>  
  
<span data-ttu-id="2bc11-124">hello är som satser bearbetas:</span><span class="sxs-lookup"><span data-stu-id="2bc11-124">hello order in which clauses are processed is:</span></span>  

1.  [<span data-ttu-id="2bc11-125">FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-125">FROM clause</span></span>](#bk_from_clause)  
2.  [<span data-ttu-id="2bc11-126">WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-126">WHERE clause</span></span>](#bk_where_clause)  
3.  [<span data-ttu-id="2bc11-127">ORDER BY-sats</span><span class="sxs-lookup"><span data-stu-id="2bc11-127">ORDER BY clause</span></span>](#bk_orderby_clause)  
4.  [<span data-ttu-id="2bc11-128">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-128">SELECT clause</span></span>](#bk_select_query)  

<span data-ttu-id="2bc11-129">Observera att detta skiljer sig från hello ordning som de visas i hello syntax.</span><span class="sxs-lookup"><span data-stu-id="2bc11-129">Note that this is different from hello order in which they appear in hello syntax.</span></span> <span data-ttu-id="2bc11-130">hello ordning är så att alla nya symboler som introducerades av en bearbetade sats visas och kan användas i satser bearbetas senare.</span><span class="sxs-lookup"><span data-stu-id="2bc11-130">hello ordering is such that all new symbols introduced by a processed clause are visible and can be used in clauses processed later.</span></span> <span data-ttu-id="2bc11-131">Till exempel alias som deklareras i en FROM-sats är tillgängliga i var och SELECT-satser.</span><span class="sxs-lookup"><span data-stu-id="2bc11-131">For instance, aliases declared in a FROM clause are accessible in WHERE and SELECT clauses.</span></span>  

<span data-ttu-id="2bc11-132">**Tecken som blanksteg och kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-132">**Whitespace characters and comments**</span></span>  

<span data-ttu-id="2bc11-133">Alla blanktecken som inte är del av en sträng inom citattecken eller identifierare med citattecken är inte en del av hello språk grammatik och ignoreras vid parsning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-133">All white space characters which are not part of a quoted string or quoted identifier are not part of hello language grammar and are ignored during parsing.</span></span>  

<span data-ttu-id="2bc11-134">kommentarer för T-SQL-format som har stöd för hello-frågespråket</span><span class="sxs-lookup"><span data-stu-id="2bc11-134">hello query language supports T-SQL style comments like</span></span>  

-   <span data-ttu-id="2bc11-135">SQL-uttryck`-- comment text [newline]`</span><span class="sxs-lookup"><span data-stu-id="2bc11-135">SQL Statement `-- comment text [newline]`</span></span>  

<span data-ttu-id="2bc11-136">Medan tecken som blanksteg och kommentarer inte har någon betydelse i hello grammatik, måste de vara används tooseparate token.</span><span class="sxs-lookup"><span data-stu-id="2bc11-136">While whitespace characters and comments do not have any significance in hello grammar, they must be used tooseparate tokens.</span></span> <span data-ttu-id="2bc11-137">Exempel: `-1e5` är ett enda nummer token, tag`: – 1 e5` följs minus token av nummer 1 och identifierare e5.</span><span class="sxs-lookup"><span data-stu-id="2bc11-137">For instance: `-1e5` is a single number token, while`: – 1 e5` is a minus token followed by number 1 and identifier e5.</span></span>  

##  <span data-ttu-id="2bc11-138"><a name="bk_select_query"></a>SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-138"><a name="bk_select_query"></a> SELECT clause</span></span>  
<span data-ttu-id="2bc11-139">hello-satser i hello SELECT-instruktion måste beställas som ovan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-139">hello clauses in hello SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="2bc11-140">En av valfri hello-satser kan utelämnas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-140">Any one of hello optional clauses can be omitted.</span></span> <span data-ttu-id="2bc11-141">Men när valfria satser används, måste de visas i hello rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-141">But when optional clauses are used, they must appear in hello right order.</span></span>  

<span data-ttu-id="2bc11-142">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-142">**Syntax**</span></span>  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 <span data-ttu-id="2bc11-143">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-143">**Arguments**</span></span>  
  
 `<select_specification>`  
  
 <span data-ttu-id="2bc11-144">Ange egenskaper eller värdet toobe som valts för hello resultat.</span><span class="sxs-lookup"><span data-stu-id="2bc11-144">Properties or value toobe selected for hello result set.</span></span>  
  
 `'*'`  
  
<span data-ttu-id="2bc11-145">Anger att hello ska hämtas utan ändringar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-145">Specifies that hello value should be retrieved without making any changes.</span></span> <span data-ttu-id="2bc11-146">Mer specifikt om hello bearbetas värdet är ett objekt, hämtas alla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2bc11-146">Specifically if hello processed value is an object, all properties will be retrieved.</span></span>  
  
 `<object_property_list>`  
  
<span data-ttu-id="2bc11-147">Anger hello lista över egenskaper toobe hämtas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-147">Specifies hello list of properties toobe retrieved.</span></span> <span data-ttu-id="2bc11-148">Varje returnerade värdet ska vara ett objekt med hello egenskaper som anges.</span><span class="sxs-lookup"><span data-stu-id="2bc11-148">Each returned value will be an object with hello properties specified.</span></span>  
  
`VALUE`  
  
<span data-ttu-id="2bc11-149">Anger att hello JSON-värde ska hämtas i stället för hello fullständig JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-149">Specifies that hello JSON value should be retrieved instead of hello complete JSON object.</span></span> <span data-ttu-id="2bc11-150">Detta, till skillnad från `<property_list>` radbryts inte hello planerat värde i ett objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-150">This, unlike `<property_list>` does not wrap hello projected value in an object.</span></span>  
  
`<scalar_expression>`  
  
<span data-ttu-id="2bc11-151">Uttryck som representerar hello värdet toobe beräknas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-151">Expression representing hello value toobe computed.</span></span> <span data-ttu-id="2bc11-152">Se [skaläruttryck](#bk_scalar_expressions) information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-152">See [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
<span data-ttu-id="2bc11-153">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-153">**Remarks**</span></span>  
  
<span data-ttu-id="2bc11-154">Hej `SELECT *` syntax är bara giltigt om FROM-satsen har deklarerats exakt ett alias.</span><span class="sxs-lookup"><span data-stu-id="2bc11-154">hello `SELECT *` syntax is only valid if FROM clause has declared exactly one alias.</span></span> <span data-ttu-id="2bc11-155">`SELECT *`innehåller en identity-projektion som kan vara användbar om det behövs ingen projektion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-155">`SELECT *` provides an identity projection, which can be useful if no projection is needed.</span></span> <span data-ttu-id="2bc11-156">Välj * är bara giltigt om FROM-satsen har angetts och införs bara en enda Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-156">SELECT * is only valid if FROM clause is specified and introduced only a single input source.</span></span>  
  
<span data-ttu-id="2bc11-157">Observera att `SELECT <select_list>` och `SELECT *` är ”syntaktiska socker” och kan uttryckas också med hjälp av enkla SELECT-satser som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-157">Note that `SELECT <select_list>` and `SELECT *` are "syntactic sugar" and can be alternatively expressed by using simple SELECT statements as shown below.</span></span>  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     <span data-ttu-id="2bc11-158">motsvarar:</span><span class="sxs-lookup"><span data-stu-id="2bc11-158">is equivalent to:</span></span>  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     <span data-ttu-id="2bc11-159">motsvarar:</span><span class="sxs-lookup"><span data-stu-id="2bc11-159">is equivalent to:</span></span>  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
<span data-ttu-id="2bc11-160">**Se även**</span><span class="sxs-lookup"><span data-stu-id="2bc11-160">**See Also**</span></span>  
  
[<span data-ttu-id="2bc11-161">Skalära uttryck</span><span class="sxs-lookup"><span data-stu-id="2bc11-161">Scalar expressions</span></span>](#bk_scalar_expressions)  
[<span data-ttu-id="2bc11-162">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-162">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="2bc11-163"><a name="bk_from_clause"></a>FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-163"><a name="bk_from_clause"></a> FROM clause</span></span>  
<span data-ttu-id="2bc11-164">Anger hello käll- eller anslutna källor.</span><span class="sxs-lookup"><span data-stu-id="2bc11-164">Specifies hello source or joined sources.</span></span> <span data-ttu-id="2bc11-165">hello FROM-satsen är valfritt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-165">hello FROM clause is optional.</span></span> <span data-ttu-id="2bc11-166">Om inte angivna, andra satser körs fortfarande som om FROM-satsen som ett enskilt dokument.</span><span class="sxs-lookup"><span data-stu-id="2bc11-166">If not specified, other clauses will still be executed as if FROM clause provided a single document.</span></span>  
  
<span data-ttu-id="2bc11-167">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-167">**Syntax**</span></span>  
  
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
  
<span data-ttu-id="2bc11-168">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-168">**Arguments**</span></span>  
  
`<from_source>`  
  
<span data-ttu-id="2bc11-169">Anger en datakälla med eller utan ett alias.</span><span class="sxs-lookup"><span data-stu-id="2bc11-169">Specifies a data source, with or without an alias.</span></span> <span data-ttu-id="2bc11-170">Om alias inte anges kommer den härledas från hello `<collection_expression>` med hjälp av följande regler:</span><span class="sxs-lookup"><span data-stu-id="2bc11-170">If alias is not specified, it will be inferred from hello `<collection_expression>` using following rules:</span></span>  
  
-   <span data-ttu-id="2bc11-171">Om hello-uttrycket är ett samlingsnamn, kommer samlingsnamn användas som ett alias.</span><span class="sxs-lookup"><span data-stu-id="2bc11-171">If hello expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
-   <span data-ttu-id="2bc11-172">Om hello-uttrycket är `<collection_expression>`, och sedan property_name sedan property_name kommer att användas som ett alias.</span><span class="sxs-lookup"><span data-stu-id="2bc11-172">If hello expression is `<collection_expression>`, then property_name, then property_name will be used as an alias.</span></span> <span data-ttu-id="2bc11-173">Om hello-uttrycket är ett samlingsnamn, kommer samlingsnamn användas som ett alias.</span><span class="sxs-lookup"><span data-stu-id="2bc11-173">If hello expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
<span data-ttu-id="2bc11-174">SOM`input_alias`</span><span class="sxs-lookup"><span data-stu-id="2bc11-174">AS `input_alias`</span></span>  
  
<span data-ttu-id="2bc11-175">Anger att hello `input_alias` är en uppsättning värden som returneras av hello underliggande samling uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-175">Specifies that hello `input_alias` is a set of values returned by hello underlying collection expression.</span></span>  
 
<span data-ttu-id="2bc11-176">`input_alias`I</span><span class="sxs-lookup"><span data-stu-id="2bc11-176">`input_alias` IN</span></span>  
  
<span data-ttu-id="2bc11-177">Anger att hello `input_alias` bör vara hello uppsättning värden som hämtas av iterera över alla matriselement för varje matrisen som returneras av hello underliggande samling uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-177">Specifies that hello `input_alias` should represent hello set of values obtained by iterating over all array elements of each array returned by hello underlying collection expression.</span></span> <span data-ttu-id="2bc11-178">Alla värden som returneras av underliggande samling uttryck som inte är en objektmatris ignoreras.</span><span class="sxs-lookup"><span data-stu-id="2bc11-178">Any value returned by underlying collection expression that is not an array is ignored.</span></span>  
  
`<collection_expression>`  
  
<span data-ttu-id="2bc11-179">Anger hello samling uttryck toobe tooretrieve hello dokument.</span><span class="sxs-lookup"><span data-stu-id="2bc11-179">Specifies hello collection expression toobe used tooretrieve hello documents.</span></span>  
  
`ROOT`  
  
<span data-ttu-id="2bc11-180">Anger att dokumentet ska hämtas från hello standard anslutna samling.</span><span class="sxs-lookup"><span data-stu-id="2bc11-180">Specifies that document should be retrieved from hello default, currently connected collection.</span></span>  
  
`collection_name`  
  
<span data-ttu-id="2bc11-181">Anger att dokumentet ska hämtas från hello tillhandahålls samling.</span><span class="sxs-lookup"><span data-stu-id="2bc11-181">Specifies that document should be retrieved from hello provided collection.</span></span> <span data-ttu-id="2bc11-182">hello namnet hello mängden måste matcha hello namnet på hello samlingen som är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="2bc11-182">hello name of hello collection must match hello name of hello collection currently connected to.</span></span>  
  
`input_alias`  
  
<span data-ttu-id="2bc11-183">Anger att dokumentet ska hämtas från hello annan källa som definieras av hello som alias.</span><span class="sxs-lookup"><span data-stu-id="2bc11-183">Specifies that document should be retrieved from hello other source defined by hello provided alias.</span></span>  
  
`<collection_expression> '.' property_`  
  
<span data-ttu-id="2bc11-184">Anger att dokumentet ska hämtas genom att öppna hello `property_name` egenskap eller array_index matriselement för alla dokument som hämtas av angivna samlingsuttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-184">Specifies that document should be retrieved by accessing hello `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
<span data-ttu-id="2bc11-185">Anger att dokumentet ska hämtas genom att öppna hello `property_name` egenskap eller array_index matriselement för alla dokument som hämtas av angivna samlingsuttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-185">Specifies that document should be retrieved by accessing hello `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
<span data-ttu-id="2bc11-186">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-186">**Remarks**</span></span>  
  
<span data-ttu-id="2bc11-187">Alla alias tillhandahålls eller härleda i hello `<from_source>(`s) måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="2bc11-187">All aliases provided or inferred in hello `<from_source>(`s) must be unique.</span></span> <span data-ttu-id="2bc11-188">hello Syntax `<collection_expression>.`property_name är hello samma som `<collection_expression>' ['"property_name"']'`.</span><span class="sxs-lookup"><span data-stu-id="2bc11-188">hello Syntax `<collection_expression>.`property_name is hello same as `<collection_expression>' ['"property_name"']'`.</span></span> <span data-ttu-id="2bc11-189">Hello senare syntaxen kan dock användas om ett egenskapsnamn som innehåller en icke-ID-tecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-189">However, hello latter syntax can be used if a property name contains a non-identifier characters.</span></span>  
  
<span data-ttu-id="2bc11-190">**Saknar egenskaper, saknas matriselement, Odefinierad värden hantering**</span><span class="sxs-lookup"><span data-stu-id="2bc11-190">**Missing properties, missing array elements, undefined values handling**</span></span>  
  
<span data-ttu-id="2bc11-191">Om ett uttryck för samlingen använder egenskaper eller array-element och att värdet inte finns kan ignoreras värdet och inte bearbetas ytterligare.</span><span class="sxs-lookup"><span data-stu-id="2bc11-191">If a collection expression accesses properties or array elements and that value does not exist, that value will be ignored and not processed further.</span></span>  
  
<span data-ttu-id="2bc11-192">**Omfattningen för samlingen uttryck kontext**</span><span class="sxs-lookup"><span data-stu-id="2bc11-192">**Collection expression context scoping**</span></span>  
  
<span data-ttu-id="2bc11-193">En samling uttryck kan vara samling omfång eller dokumentet omfattar:</span><span class="sxs-lookup"><span data-stu-id="2bc11-193">A collection expression may be collection-scoped or document-scoped:</span></span>  
  
-   <span data-ttu-id="2bc11-194">Ett uttryck som är begränsad samling, om hello underliggande för hello samling uttryck är antingen rot eller `collection_name`.</span><span class="sxs-lookup"><span data-stu-id="2bc11-194">An expression is collection-scoped, if hello underlying source of hello collection expression is either ROOT or `collection_name`.</span></span> <span data-ttu-id="2bc11-195">Dessa uttryck representerar en uppsättning dokument som hämtats från hello samling direkt och är inte beroende av hello bearbetning av andra uttryck för samlingen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-195">Such an expression represents a set of documents retrieved from hello collection directly, and is not dependent on hello processing of other collection expressions.</span></span>  
  
-   <span data-ttu-id="2bc11-196">Ett uttryck är dokumentet omfång, om hello underliggande för hello samling uttryck är `input_alias` tidigare lanserades hello frågan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-196">An expression is document-scoped, if hello underlying source of hello collection expression is `input_alias` introduced earlier in hello query.</span></span> <span data-ttu-id="2bc11-197">Dessa uttryck representerar en uppsättning dokument som erhålls genom att utvärdera hello samlingsuttrycket i hello omfång för varje dokument som tillhör toohello set som är associerade med samlingen för hello-alias.</span><span class="sxs-lookup"><span data-stu-id="2bc11-197">Such an expression represents a set of documents obtained by evaluating hello collection expression in hello scope of each document belonging toohello set associated with hello aliased collection.</span></span>  <span data-ttu-id="2bc11-198">hello resultatmängden blir en union av mängder som erhålls genom att utvärdera hello samlingsuttrycket för varje hello dokument i hello underliggande uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-198">hello resulting set will be a union of sets obtained by evaluating hello collection expression for each of hello documents in hello underlying set.</span></span>  
  
<span data-ttu-id="2bc11-199">**Kopplingar**</span><span class="sxs-lookup"><span data-stu-id="2bc11-199">**Joins**</span></span>  
  
<span data-ttu-id="2bc11-200">I hello aktuella versionen stöder Azure Cosmos DB inre kopplingar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-200">In hello current release, Azure Cosmos DB supports inner joins.</span></span> <span data-ttu-id="2bc11-201">Anslut till ytterligare funktioner är kommande.</span><span class="sxs-lookup"><span data-stu-id="2bc11-201">Additional join capabilities are forthcoming.</span></span>

<span data-ttu-id="2bc11-202">Inre kopplingar i en fullständig kryssprodukten av hello resultatuppsättningar deltar i hello koppling.</span><span class="sxs-lookup"><span data-stu-id="2bc11-202">Inner joins result in a complete cross product of hello sets participating in hello join.</span></span> <span data-ttu-id="2bc11-203">hello resultatet av en N-vägs-koppling är en tuppelmängd N-element, där varje värde i hello tuppel är associerad med hello-alias som deltar i hello koppling och kan nås av refererar till detta alias i andra-satser.</span><span class="sxs-lookup"><span data-stu-id="2bc11-203">hello result of an N-way join is a set of N-element tuples, where each value in hello tuple is associated with hello aliased set participating in hello join and can be accessed by referencing that alias in other clauses.</span></span>  
  
<span data-ttu-id="2bc11-204">hello utvärdering av hello koppling beror på hello kontexten omfattning hello deltar anger:</span><span class="sxs-lookup"><span data-stu-id="2bc11-204">hello evaluation of hello join depends on hello context scope of hello participating sets:</span></span>  
  
-  <span data-ttu-id="2bc11-205">En koppling mellan samlingsuppsättningen A och samling omfång ange B, resulterar i en kryssprodukten för alla element i anger A och b</span><span class="sxs-lookup"><span data-stu-id="2bc11-205">A join between collection-set A and collection-scoped set B, results in a cross product of all elements in sets A and B.</span></span>
  
-   <span data-ttu-id="2bc11-206">En koppling mellan uppsättning A och dokumentet omfång B, som resulterar i en union av alla uppsättningar som erhålls genom att utvärdera dokument-omfattande uppsättning B för varje dokument från ange A.</span><span class="sxs-lookup"><span data-stu-id="2bc11-206">A join between set A and document-scoped set B, results in a union of all sets obtained by evaluating document-scoped set B for each document from set A.</span></span>  
  
 <span data-ttu-id="2bc11-207">I hello aktuella versionen kan stöds högst ett omfång samling uttryck av hello frågeprocessorn.</span><span class="sxs-lookup"><span data-stu-id="2bc11-207">In hello current release, a maximum of one collection-scoped expression is supported by hello query processor.</span></span>  
  
<span data-ttu-id="2bc11-208">**Exempel på kopplingar:**</span><span class="sxs-lookup"><span data-stu-id="2bc11-208">**Examples of joins:**</span></span>  
  
<span data-ttu-id="2bc11-209">Nu ska vi titta på hello efter FROM-satsen:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span><span class="sxs-lookup"><span data-stu-id="2bc11-209">Let's look at hello following FROM clause: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span></span>  
  
 <span data-ttu-id="2bc11-210">Låt varje källa definiera `input_alias1, input_alias2, …, input_aliasN`.</span><span class="sxs-lookup"><span data-stu-id="2bc11-210">Let each source define `input_alias1, input_alias2, …, input_aliasN`.</span></span> <span data-ttu-id="2bc11-211">FROM-satsen returnerar en mängd med N-tupplar (tuppel med N värden).</span><span class="sxs-lookup"><span data-stu-id="2bc11-211">This FROM clause returns a set of N-tuples (tuple with N values).</span></span> <span data-ttu-id="2bc11-212">Varje tuppel har värden som genereras av alla samling alias iterera över sina respektive uppsättningar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-212">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span>  
  
<span data-ttu-id="2bc11-213">*Gå med i exempel 1, med 2 källor:*</span><span class="sxs-lookup"><span data-stu-id="2bc11-213">*JOIN example 1, with 2 sources:*</span></span>  
  
- <span data-ttu-id="2bc11-214">Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="2bc11-214">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="2bc11-215">Låt `<from_source2>` vara dokumentet definitionsområde refererar till input_alias1 och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="2bc11-215">Let `<from_source2>` be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="2bc11-216">{1, 2} för`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-216">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="2bc11-217">{3} för`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-217">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="2bc11-218">{4, 5} för`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-218">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="2bc11-219">Hej FROM-satsen `<from_source1> JOIN <from_source2>` resulterar i följande tupplar hello:</span><span class="sxs-lookup"><span data-stu-id="2bc11-219">hello FROM clause `<from_source1> JOIN <from_source2>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="2bc11-220">(`input_alias1, input_alias2`):</span><span class="sxs-lookup"><span data-stu-id="2bc11-220">(`input_alias1, input_alias2`):</span></span>  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
<span data-ttu-id="2bc11-221">*Ansluta till exempel 2, med 3 källor:*</span><span class="sxs-lookup"><span data-stu-id="2bc11-221">*JOIN example 2, with 3 sources:*</span></span>  
  
- <span data-ttu-id="2bc11-222">Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="2bc11-222">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="2bc11-223">Låt `<from_source2>` vara omfång dokumentet refererar till `input_alias1` och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="2bc11-223">Let `<from_source2>` be document-scoped referencing `input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="2bc11-224">{1, 2} för`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-224">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="2bc11-225">{3} för`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-225">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="2bc11-226">{4, 5} för`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-226">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="2bc11-227">Låt `<from_source3>` vara omfång dokumentet refererar till `input_alias2` och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="2bc11-227">Let `<from_source3>` be document-scoped referencing `input_alias2` and represent sets:</span></span>  
  
    <span data-ttu-id="2bc11-228">{100, 200} för`input_alias2 = 1,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-228">{100, 200} for `input_alias2 = 1,`</span></span>  
  
    <span data-ttu-id="2bc11-229">{300} för`input_alias2 = 3,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-229">{300} for `input_alias2 = 3,`</span></span>  
  
- <span data-ttu-id="2bc11-230">Hej FROM-satsen `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulterar i följande tupplar hello:</span><span class="sxs-lookup"><span data-stu-id="2bc11-230">hello FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="2bc11-231">(input_alias1, input_alias2, input_alias3):</span><span class="sxs-lookup"><span data-stu-id="2bc11-231">(input_alias1, input_alias2, input_alias3):</span></span>  
  
    <span data-ttu-id="2bc11-232">A-, 1, 100 (A, 1, 200), (B, 3, 300)</span><span class="sxs-lookup"><span data-stu-id="2bc11-232">(A, 1, 100), (A, 1, 200), (B, 3, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="2bc11-233">Avsaknad av tupplar för andra `input_alias1`, `input_alias2`, för vilka hello `<from_source3>` returnerade inte några värden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-233">Lack of tuples for other values of `input_alias1`, `input_alias2`, for which hello `<from_source3>` did not return any values.</span></span>  
  
<span data-ttu-id="2bc11-234">*Ansluta till exempel 3, med 3 källor:*</span><span class="sxs-lookup"><span data-stu-id="2bc11-234">*JOIN example 3, with 3 sources:*</span></span>  
  
- <span data-ttu-id="2bc11-235">Låt < from_source1 > vara begränsad samling som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="2bc11-235">Let <from_source1> be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="2bc11-236">Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="2bc11-236">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="2bc11-237">Låt < from_source2 > att dokumentet omfång refererande input_alias1 och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="2bc11-237">Let <from_source2> be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="2bc11-238">{1, 2} för`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-238">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="2bc11-239">{3} för`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-239">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="2bc11-240">{4, 5} för`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-240">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="2bc11-241">Låt `<from_source3>` begränsas för`input_alias1` och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="2bc11-241">Let `<from_source3>` be scoped too`input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="2bc11-242">{100, 200} för`input_alias2 = A,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-242">{100, 200} for `input_alias2 = A,`</span></span>  
  
    <span data-ttu-id="2bc11-243">{300} för`input_alias2 = C,`</span><span class="sxs-lookup"><span data-stu-id="2bc11-243">{300} for `input_alias2 = C,`</span></span>  
  
- <span data-ttu-id="2bc11-244">Hej FROM-satsen `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulterar i följande tupplar hello:</span><span class="sxs-lookup"><span data-stu-id="2bc11-244">hello FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in hello following tuples:</span></span>  
  
    <span data-ttu-id="2bc11-245">(`input_alias1, input_alias2, input_alias3`):</span><span class="sxs-lookup"><span data-stu-id="2bc11-245">(`input_alias1, input_alias2, input_alias3`):</span></span>  
  
    <span data-ttu-id="2bc11-246">A-, 1, 100 (A, 1, 200), A-, 2, 100 A-, 2, 200 C, 4, 300, (C, 5, 300)</span><span class="sxs-lookup"><span data-stu-id="2bc11-246">(A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200),  (C, 4, 300) ,  (C, 5, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="2bc11-247">Detta resulterade i kryssprodukten mellan `<from_source2>` och `<from_source3>` eftersom båda är begränsade toohello samma `<from_source1>`.</span><span class="sxs-lookup"><span data-stu-id="2bc11-247">This resulted in cross product between `<from_source2>` and `<from_source3>` because both are scoped toohello same `<from_source1>`.</span></span>  <span data-ttu-id="2bc11-248">Detta resulterade i 4.2 2 x tupplar med värdet A, 0 tupplar med värdet B (1 x 0) och 2 (2 x 1) tupplar med värdet C.</span><span class="sxs-lookup"><span data-stu-id="2bc11-248">This resulted in 4 (2x2) tuples having value A, 0 tuples having value B (1x0) and 2 (2x1) tuples having value C.</span></span>  
  
<span data-ttu-id="2bc11-249">**Se även**</span><span class="sxs-lookup"><span data-stu-id="2bc11-249">**See also**</span></span>  
  
 [<span data-ttu-id="2bc11-250">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-250">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="2bc11-251"><a name="bk_where_clause"></a>WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="2bc11-251"><a name="bk_where_clause"></a> WHERE clause</span></span>  
 <span data-ttu-id="2bc11-252">Anger hello sökvillkor för hello dokument som returneras av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-252">Specifies hello search condition for hello documents returned by hello query.</span></span>  
  
 <span data-ttu-id="2bc11-253">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-253">**Syntax**</span></span>  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 <span data-ttu-id="2bc11-254">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-254">**Arguments**</span></span>  
  
-   `<filter_condition>`  
  
     <span data-ttu-id="2bc11-255">Anger hello villkoret toobe för hello dokument toobe returneras.</span><span class="sxs-lookup"><span data-stu-id="2bc11-255">Specifies hello condition toobe met for hello documents toobe returned.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="2bc11-256">Uttryck som representerar hello värdet toobe beräknas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-256">Expression representing hello value toobe computed.</span></span> <span data-ttu-id="2bc11-257">Se hello [skaläruttryck](#bk_scalar_expressions) information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-257">See hello [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
 <span data-ttu-id="2bc11-258">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-258">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-259">För att hello returnerade dokumentet toobe ett uttryck som angetts som filtervillkor måste utvärderas tootrue.</span><span class="sxs-lookup"><span data-stu-id="2bc11-259">In order for hello document toobe returned an expression specified as filter condition must evaluate tootrue.</span></span> <span data-ttu-id="2bc11-260">Endast booleska värdet true ska uppfylla hello villkor och ett annat värde: Odefinierad, null, false, antalet, matris eller ett objekt ska inte uppfyller hello villkor.</span><span class="sxs-lookup"><span data-stu-id="2bc11-260">Only Boolean value true will satisfy hello condition, any other value: undefined, null, false, Number, Array or Object will not satisfy hello condition.</span></span>  
  
##  <span data-ttu-id="2bc11-261"><a name="bk_orderby_clause"></a>ORDER BY-sats</span><span class="sxs-lookup"><span data-stu-id="2bc11-261"><a name="bk_orderby_clause"></a> ORDER BY clause</span></span>  
 <span data-ttu-id="2bc11-262">Anger hello sorteringsordning för resultaten som returnerades av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-262">Specifies hello sorting order for results returned by hello query.</span></span>  
  
 <span data-ttu-id="2bc11-263">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-263">**Syntax**</span></span>  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 <span data-ttu-id="2bc11-264">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-264">**Arguments**</span></span>  
  
-   `<sort_specification>`  
  
     <span data-ttu-id="2bc11-265">Anger en egenskap eller ett uttryck som toosort hello frågan resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-265">Specifies a property or expression on which toosort hello query result set.</span></span> <span data-ttu-id="2bc11-266">En sorteringskolumn kan anges som ett alias eller en kolumn.</span><span class="sxs-lookup"><span data-stu-id="2bc11-266">A sort column can be specified as a name or column alias.</span></span>  
  
     <span data-ttu-id="2bc11-267">Du kan ange flera sorteringskolumner.</span><span class="sxs-lookup"><span data-stu-id="2bc11-267">Multiple sort columns can be specified.</span></span> <span data-ttu-id="2bc11-268">Kolumnnamnen måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="2bc11-268">Column names must be unique.</span></span> <span data-ttu-id="2bc11-269">hello sifferföljd hello sorteringskolumner i hello ORDER BY-sats definierar hello organisation av hello sorteras resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-269">hello sequence of hello sort columns in hello ORDER BY clause defines hello organization of hello sorted result set.</span></span> <span data-ttu-id="2bc11-270">Att hello resultatet sorteras efter första hello-egenskapen och sedan den beställda listan sorteras efter andra hello-egenskapen och så vidare.</span><span class="sxs-lookup"><span data-stu-id="2bc11-270">That is, hello result set is sorted by hello first property and then that ordered list is sorted by hello second property, and so on.</span></span>  
  
     <span data-ttu-id="2bc11-271">hello kolumnnamn som refereras i hello ORDER BY-satsen måste matcha tooeither en kolumn i hello Markera lista eller tooa kolumn som definierats i en tabell som har angetts i hello FROM-sats utan någon tvetydigheter.</span><span class="sxs-lookup"><span data-stu-id="2bc11-271">hello column names referenced in hello ORDER BY clause must correspond tooeither a column in hello select list or tooa column defined in a table specified in hello FROM clause without any ambiguities.</span></span>  
  
-   `<sort_expression>`  
  
     <span data-ttu-id="2bc11-272">Anger en enskild egenskap eller ett uttryck på vilka toosort hello frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-272">Specifies a single property or expression on which toosort hello query result set.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="2bc11-273">Se hello [skaläruttryck](#bk_scalar_expressions) information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-273">See hello [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
-   `ASC | DESC`  
  
     <span data-ttu-id="2bc11-274">Anger att hello värden i hello angiven kolumn ska sorteras i stigande eller fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-274">Specifies that hello values in hello specified column should be sorted in ascending or descending order.</span></span> <span data-ttu-id="2bc11-275">ASC sorterar från hello lägsta värde toohighest värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-275">ASC sorts from hello lowest value toohighest value.</span></span> <span data-ttu-id="2bc11-276">DESC sorterar från högsta värde toolowest värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-276">DESC sorts from highest value toolowest value.</span></span> <span data-ttu-id="2bc11-277">ASC är hello standardsorteringsordning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-277">ASC is hello default sort order.</span></span> <span data-ttu-id="2bc11-278">Null-värden behandlas som hello lägsta möjliga värden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-278">Null values are treated as hello lowest possible values.</span></span>  
  
 <span data-ttu-id="2bc11-279">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-279">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-280">Medan hello frågegrammatik stöder flera ordning efter egenskaper, stöder hello Azure Cosmos DB frågan runtime sortering bara mot en enskild egenskap och bara mot egenskapsnamn, d.v.s. inte mot beräknade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2bc11-280">While hello query grammar supports multiple order by properties, hello Azure Cosmos DB query runtime supports sorting only against a single property, and only against property names, i.e., not against computed properties.</span></span> <span data-ttu-id="2bc11-281">Sortering kräver också att hello indexering principen innehåller ett intervall index för hello egenskap och hello anges typ med hello högsta precision.</span><span class="sxs-lookup"><span data-stu-id="2bc11-281">Sorting also requires that hello indexing policy includes a range index for hello property and hello specified type, with hello maximum precision.</span></span> <span data-ttu-id="2bc11-282">Läs toohello indexprincipdokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-282">Refer toohello indexing policy documentation for more details.</span></span>  
  
##  <span data-ttu-id="2bc11-283"><a name="bk_scalar_expressions"></a>Skalära uttryck</span><span class="sxs-lookup"><span data-stu-id="2bc11-283"><a name="bk_scalar_expressions"></a> Scalar expressions</span></span>  
 <span data-ttu-id="2bc11-284">Ett skalärt uttryck som är en kombination av symboler operatorer som kan utvärderas tooobtain ett enskilt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-284">A scalar expression is a combination of symbols and operators that can be evaluated tooobtain a single value.</span></span> <span data-ttu-id="2bc11-285">Enkla uttryck kan vara konstanter, egenskapsreferenser, matris referenser, alias referenser eller funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="2bc11-285">Simple expressions can be constants, property references, array element references, alias references, or function calls.</span></span> <span data-ttu-id="2bc11-286">Enkla uttryck kan kombineras till komplexa uttryck med hjälp av operatörer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-286">Simple expressions can be combined into complex expressions using operators.</span></span>  
  
 <span data-ttu-id="2bc11-287">Mer information om vilka skalärt uttryck som kan ha värden finns [konstanter](#bk_constants) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-287">For details on values which scalar expression may have, see [Constants](#bk_constants) section.</span></span>  
  
 <span data-ttu-id="2bc11-288">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-288">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="2bc11-289">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-289">**Arguments**</span></span>  
  
-   `<constant>`  
  
     <span data-ttu-id="2bc11-290">Representerar ett konstant värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-290">Represents a constant value.</span></span> <span data-ttu-id="2bc11-291">Se [konstanter](#bk_constants) information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-291">See [Constants](#bk_constants) section for details.</span></span>  
  
-   `input_alias`  
  
     <span data-ttu-id="2bc11-292">Representerar ett värde som definierats av hello `input_alias` introducerades i hello `FROM` satsen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-292">Represents a value defined by hello `input_alias` introduced in hello `FROM` clause.</span></span>  
    <span data-ttu-id="2bc11-293">Det här värdet är garanterat toonot vara **Odefinierad** –**Odefinierad** värden i hello indata hoppas över.</span><span class="sxs-lookup"><span data-stu-id="2bc11-293">This value is guaranteed toonot be **undefined** –**undefined** values in hello input are skipped.</span></span>  
  
-   `<scalar_expression>.property_name`  
  
     <span data-ttu-id="2bc11-294">Representerar ett värde för hello-egenskapen för ett objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-294">Represents a value of hello property of an object.</span></span> <span data-ttu-id="2bc11-295">Om hello egenskapen finns inte eller egenskapen refererar till ett värde som inte är ett objekt och sedan hello uttrycket utvärderas för**Odefinierad** värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-295">If hello property does not exist or property is referenced on a value which is not an object, then hello expression evaluates too**undefined** value.</span></span>  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     <span data-ttu-id="2bc11-296">Representerar ett värde för hello egenskap med namnet `property_name` eller array-elementet med index `array_index` av en objektmatris.</span><span class="sxs-lookup"><span data-stu-id="2bc11-296">Represents a value of hello property with name `property_name` or array element with index `array_index` of an object/array.</span></span> <span data-ttu-id="2bc11-297">Om hello egenskapen/matrisindex finns inte eller hello egenskapen/matrisindex refererar till ett värde som inte är en objektmatris, utvärderar hello uttryck tooundefined värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-297">If hello property/array index does not exist or hello property/array index is referenced on a value which is not an object/array, then hello expression evaluates tooundefined value.</span></span>  
  
-   `unary_operator <scalar_expression>`  
  
     <span data-ttu-id="2bc11-298">Representerar en operator som används tooa enstaka värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-298">Represents an operator that is applied tooa single value.</span></span> <span data-ttu-id="2bc11-299">Se [operatörer](#bk_operators) information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-299">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     <span data-ttu-id="2bc11-300">Representerar en operator som är kopplade tootwo värden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-300">Represents an operator that is applied tootwo values.</span></span> <span data-ttu-id="2bc11-301">Se [operatörer](#bk_operators) information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-301">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_function_expression>`  
  
     <span data-ttu-id="2bc11-302">Representerar ett värde som definieras av ett resultat av ett funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="2bc11-302">Represents a value defined by a result of a function call.</span></span>  
  
-   `udf_scalar_function`  
  
     <span data-ttu-id="2bc11-303">Namnet på hello användare definierats skalärfunktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-303">Name of hello user defined scalar function.</span></span>  
  
-   `builtin_scalar_function`  
  
     <span data-ttu-id="2bc11-304">Namnet på hello inbyggda skalärfunktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-304">Name of hello built-in scalar function.</span></span>  
  
-   `<create_object_expression>`  
  
     <span data-ttu-id="2bc11-305">Representerar ett värde som erhålls genom att skapa ett nytt objekt med angivna egenskaper och deras värden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-305">Represents a value obtained by creating a new object with specified properties and their values.</span></span>  
  
-   `<create_array_expression>`  
  
     <span data-ttu-id="2bc11-306">Representerar ett värde som erhålls genom att skapa en ny matris med angivna värden som element</span><span class="sxs-lookup"><span data-stu-id="2bc11-306">Represents a value obtained by creating a new array with specified values as elements</span></span>  
  
-   `parameter_name`  
  
     <span data-ttu-id="2bc11-307">Representerar ett värde för hello angivna parameternamnet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-307">Represents a value of hello specified parameter name.</span></span> <span data-ttu-id="2bc11-308">Parameternamn måste ha en enda @ som hello första tecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-308">Parameter names must have a single @ as hello first character.</span></span>  
  
 <span data-ttu-id="2bc11-309">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-309">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-310">När du anropar en inbyggd eller användaren definierat skalärfunktion måste alla argument anges.</span><span class="sxs-lookup"><span data-stu-id="2bc11-310">When calling a built-in or user defined scalar function all arguments must be defined.</span></span> <span data-ttu-id="2bc11-311">Om någon av hello argument är odefinierad hello funktionen kommer inte att avbrytas och hello resultatet blir odefinierad.</span><span class="sxs-lookup"><span data-stu-id="2bc11-311">If any of hello arguments is undefined, hello function will not be called and hello result will be undefined.</span></span>  
  
 <span data-ttu-id="2bc11-312">När du skapar ett objekt, en egenskap som är tilldelad odefinierat värde hoppas över och inte ingår i hello-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-312">When creating an object, any property that is assigned undefined value will be skipped and not included in hello created object.</span></span>  
  
 <span data-ttu-id="2bc11-313">När du skapar en matris, ett elementvärde som är tilldelad **Odefinierad** värdet kommer att hoppas över och inte ingår i hello skapade objektet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-313">When creating an array, any element value that is assigned **undefined** value will be skipped and not included in hello created object.</span></span> <span data-ttu-id="2bc11-314">Detta innebär att hello nästa definierade element tootake dess plats så att hello skapade matris kommer inte ha hoppas över index.</span><span class="sxs-lookup"><span data-stu-id="2bc11-314">This will cause hello next defined element tootake its place in such a way that hello created array will not have skipped indexes.</span></span>  
  
##  <span data-ttu-id="2bc11-315"><a name="bk_operators"></a>Operatörer</span><span class="sxs-lookup"><span data-stu-id="2bc11-315"><a name="bk_operators"></a> Operators</span></span>  
 <span data-ttu-id="2bc11-316">Det här avsnittet beskrivs hello stöds operatörer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-316">This section describes hello supported operators.</span></span> <span data-ttu-id="2bc11-317">Varje operatör kan vara tilldelade tooexactly en kategori.</span><span class="sxs-lookup"><span data-stu-id="2bc11-317">Each operator can be assigned tooexactly one category.</span></span>  
  
 <span data-ttu-id="2bc11-318">Se **operatorn kategorier** tabellen nedan, mer information om hantering av **Odefinierad** värden, krav för indatavärden för och hanteringen av värden med typer som inte matchar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-318">See **Operator categories** table below, for details regarding handling of **undefined** values, type requirements for input values and handling of values with not matching types.</span></span>  
  
 <span data-ttu-id="2bc11-319">**Operatorn kategorier:**</span><span class="sxs-lookup"><span data-stu-id="2bc11-319">**Operator categories:**</span></span>  
  
|<span data-ttu-id="2bc11-320">**Kategori**</span><span class="sxs-lookup"><span data-stu-id="2bc11-320">**Category**</span></span>|<span data-ttu-id="2bc11-321">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-321">**Details**</span></span>|  
|-|-|  
|<span data-ttu-id="2bc11-322">**aritmetiska**</span><span class="sxs-lookup"><span data-stu-id="2bc11-322">**arithmetic**</span></span>|<span data-ttu-id="2bc11-323">Operatorn förväntar input(s) toobe nummer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-323">Operator expects input(s) toobe Number(s).</span></span> <span data-ttu-id="2bc11-324">Utdata är också ett tal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-324">Output is also a Number.</span></span> <span data-ttu-id="2bc11-325">Om någon av hello indata är **Odefinierad** eller annan typ än antalet sedan hello resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-325">If any of hello inputs is **undefined** or type other than Number then hello result is **undefined**.</span></span>|  
|<span data-ttu-id="2bc11-326">**binär**</span><span class="sxs-lookup"><span data-stu-id="2bc11-326">**bitwise**</span></span>|<span data-ttu-id="2bc11-327">Operatorn förväntar input(s) toobe 32-bitars heltal nummer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-327">Operator expects input(s) toobe 32-bit signed integer Number(s).</span></span> <span data-ttu-id="2bc11-328">Utdata är också 32-bitars heltal nummer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-328">Output is also 32-bit signed integer Number.</span></span><br /><br /> <span data-ttu-id="2bc11-329">Att kommer avrundas valfritt heltal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-329">Any non-integer value will be rounded.</span></span> <span data-ttu-id="2bc11-330">Ett positivt värde avrundas nedåt, negativa värden avrunda uppåt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-330">Positive value will be rounded down, negative values rounded up.</span></span><br /><br /> <span data-ttu-id="2bc11-331">Ett värde som är utanför intervallet för 32-bitars heltal av hello konverteras med sista 32-bitar för dess två visas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-331">Any value that is outside of hello 32-bit integer range will be converted, by taking last 32-bits of its two's complement notation.</span></span><br /><br /> <span data-ttu-id="2bc11-332">Om någon av hello indata är **Odefinierad** eller annan typ än siffra, och sedan hello resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-332">If any of hello inputs is **undefined** or type other than Number, then hello result is **undefined**.</span></span><br /><br /> <span data-ttu-id="2bc11-333">**Obs:** hello ovan beteendet är kompatibel med JavaScript binär operator-beteende.</span><span class="sxs-lookup"><span data-stu-id="2bc11-333">**Note:** hello above behavior is compatible with JavaScript bitwise operator behavior.</span></span>|  
|<span data-ttu-id="2bc11-334">**logiska**</span><span class="sxs-lookup"><span data-stu-id="2bc11-334">**logical**</span></span>|<span data-ttu-id="2bc11-335">Operatorn förväntar input(s) toobe Boolean(s).</span><span class="sxs-lookup"><span data-stu-id="2bc11-335">Operator expects input(s) toobe Boolean(s).</span></span> <span data-ttu-id="2bc11-336">Utdata är också ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-336">Output is also a Boolean.</span></span><br /><span data-ttu-id="2bc11-337">Om någon av hello indata är **Odefinierad** eller annan typ än Boolean, och sedan hello resultatet **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-337">If any of hello inputs is **undefined** or type other than Boolean, then hello result will be **undefined**.</span></span>|  
|<span data-ttu-id="2bc11-338">**jämförelse**</span><span class="sxs-lookup"><span data-stu-id="2bc11-338">**comparison**</span></span>|<span data-ttu-id="2bc11-339">Operatorn förväntar input(s) toohave hello samma och inte är odefinierad.</span><span class="sxs-lookup"><span data-stu-id="2bc11-339">Operator expects input(s) toohave hello same type and not be undefined.</span></span> <span data-ttu-id="2bc11-340">Utdata är ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-340">Output is a Boolean.</span></span><br /><br /> <span data-ttu-id="2bc11-341">Om någon av hello indata är **Odefinierad** eller hello indata har olika typer och sedan hello resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-341">If any of hello inputs is **undefined** or hello inputs have different types, then hello result is **undefined**.</span></span><br /><br /> <span data-ttu-id="2bc11-342">Se **ordning med värden för jämförelse** tabellen för värdet ordning information.</span><span class="sxs-lookup"><span data-stu-id="2bc11-342">See **Ordering of values for comparison** table for value ordering details.</span></span>|  
|<span data-ttu-id="2bc11-343">**sträng**</span><span class="sxs-lookup"><span data-stu-id="2bc11-343">**string**</span></span>|<span data-ttu-id="2bc11-344">Operatorn förväntar input(s) toobe strängarna.</span><span class="sxs-lookup"><span data-stu-id="2bc11-344">Operator expects input(s) toobe String(s).</span></span> <span data-ttu-id="2bc11-345">Utdata är också en sträng.</span><span class="sxs-lookup"><span data-stu-id="2bc11-345">Output is also a String.</span></span><br /><span data-ttu-id="2bc11-346">Om någon av hello indata är **Odefinierad** eller annan typ än sträng sedan hello resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-346">If any of hello inputs is **undefined** or type other than String then hello result is **undefined**.</span></span>|  
  
 <span data-ttu-id="2bc11-347">**Unära operatorer:**</span><span class="sxs-lookup"><span data-stu-id="2bc11-347">**Unary operators:**</span></span>  
  
|<span data-ttu-id="2bc11-348">**Namn**</span><span class="sxs-lookup"><span data-stu-id="2bc11-348">**Name**</span></span>|<span data-ttu-id="2bc11-349">**Operatorn**</span><span class="sxs-lookup"><span data-stu-id="2bc11-349">**Operator**</span></span>|<span data-ttu-id="2bc11-350">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-350">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="2bc11-351">**aritmetiska**</span><span class="sxs-lookup"><span data-stu-id="2bc11-351">**arithmetic**</span></span>|+<br /><br /> -|<span data-ttu-id="2bc11-352">Returnerar hello numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-352">Returns hello number value.</span></span><br /><br /> <span data-ttu-id="2bc11-353">Binär negation.</span><span class="sxs-lookup"><span data-stu-id="2bc11-353">Bitwise negation.</span></span> <span data-ttu-id="2bc11-354">Returnerar negated numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-354">Returns negated number value.</span></span>|  
|<span data-ttu-id="2bc11-355">**binär**</span><span class="sxs-lookup"><span data-stu-id="2bc11-355">**bitwise**</span></span>|~|<span data-ttu-id="2bc11-356">De som har komplementet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-356">Ones' complement.</span></span> <span data-ttu-id="2bc11-357">Returnerar en uppsättning ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-357">Returns a complement of a number value.</span></span>|  
|<span data-ttu-id="2bc11-358">**Logiska**</span><span class="sxs-lookup"><span data-stu-id="2bc11-358">**Logical**</span></span>|<span data-ttu-id="2bc11-359">**INTE**</span><span class="sxs-lookup"><span data-stu-id="2bc11-359">**NOT**</span></span>|<span data-ttu-id="2bc11-360">Negation.</span><span class="sxs-lookup"><span data-stu-id="2bc11-360">Negation.</span></span> <span data-ttu-id="2bc11-361">Returnerar negated booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-361">Returns negated Boolean value.</span></span>|  
  
 <span data-ttu-id="2bc11-362">**Binära operatorer:**</span><span class="sxs-lookup"><span data-stu-id="2bc11-362">**Binary operators:**</span></span>  
  
|<span data-ttu-id="2bc11-363">**Namn**</span><span class="sxs-lookup"><span data-stu-id="2bc11-363">**Name**</span></span>|<span data-ttu-id="2bc11-364">**Operatorn**</span><span class="sxs-lookup"><span data-stu-id="2bc11-364">**Operator**</span></span>|<span data-ttu-id="2bc11-365">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-365">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="2bc11-366">**aritmetiska**</span><span class="sxs-lookup"><span data-stu-id="2bc11-366">**arithmetic**</span></span>|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|<span data-ttu-id="2bc11-367">Tillägg.</span><span class="sxs-lookup"><span data-stu-id="2bc11-367">Addition.</span></span><br /><br /> <span data-ttu-id="2bc11-368">Subtraktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-368">Subtraction.</span></span><br /><br /> <span data-ttu-id="2bc11-369">Multiplikation.</span><span class="sxs-lookup"><span data-stu-id="2bc11-369">Multiplication.</span></span><br /><br /> <span data-ttu-id="2bc11-370">Division.</span><span class="sxs-lookup"><span data-stu-id="2bc11-370">Division.</span></span><br /><br /> <span data-ttu-id="2bc11-371">Modulering.</span><span class="sxs-lookup"><span data-stu-id="2bc11-371">Modulation.</span></span>|  
|<span data-ttu-id="2bc11-372">**binär**</span><span class="sxs-lookup"><span data-stu-id="2bc11-372">**bitwise**</span></span>|<span data-ttu-id="2bc11-373">&#124;</span><span class="sxs-lookup"><span data-stu-id="2bc11-373">&#124;</span></span><br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|<span data-ttu-id="2bc11-374">Logiskt eller.</span><span class="sxs-lookup"><span data-stu-id="2bc11-374">Bitwise OR.</span></span><br /><br /> <span data-ttu-id="2bc11-375">Binär och.</span><span class="sxs-lookup"><span data-stu-id="2bc11-375">Bitwise AND.</span></span><br /><br /> <span data-ttu-id="2bc11-376">Bitvis XOR.</span><span class="sxs-lookup"><span data-stu-id="2bc11-376">Bitwise XOR.</span></span><br /><br /> <span data-ttu-id="2bc11-377">Vänsterskift.</span><span class="sxs-lookup"><span data-stu-id="2bc11-377">Left Shift.</span></span><br /><br /> <span data-ttu-id="2bc11-378">Högerskift.</span><span class="sxs-lookup"><span data-stu-id="2bc11-378">Right Shift.</span></span><br /><br /> <span data-ttu-id="2bc11-379">Noll fill högerskift.</span><span class="sxs-lookup"><span data-stu-id="2bc11-379">Zero-fill Right Shift.</span></span>|  
|<span data-ttu-id="2bc11-380">**logiska**</span><span class="sxs-lookup"><span data-stu-id="2bc11-380">**logical**</span></span>|<span data-ttu-id="2bc11-381">**OCH**</span><span class="sxs-lookup"><span data-stu-id="2bc11-381">**AND**</span></span><br /><br /> <span data-ttu-id="2bc11-382">**ELLER**</span><span class="sxs-lookup"><span data-stu-id="2bc11-382">**OR**</span></span>|<span data-ttu-id="2bc11-383">Logisk konjunktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-383">Logical conjunction.</span></span> <span data-ttu-id="2bc11-384">Returnerar **SANT** om båda argument är **SANT**, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-384">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="2bc11-385">Logisk konjunktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-385">Logical conjunction.</span></span> <span data-ttu-id="2bc11-386">Returnerar **SANT** om båda argument är **SANT**, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-386">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span>|  
|<span data-ttu-id="2bc11-387">**jämförelse**</span><span class="sxs-lookup"><span data-stu-id="2bc11-387">**comparison**</span></span>|**=**<br /><br /> <span data-ttu-id="2bc11-388">**!=, <>**</span><span class="sxs-lookup"><span data-stu-id="2bc11-388">**!=, <>**</span></span><br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> <span data-ttu-id="2bc11-389">**??**</span><span class="sxs-lookup"><span data-stu-id="2bc11-389">**??**</span></span>|<span data-ttu-id="2bc11-390">Lika med.</span><span class="sxs-lookup"><span data-stu-id="2bc11-390">Equals.</span></span> <span data-ttu-id="2bc11-391">Returnerar **SANT** om argument är lika returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-391">Returns **true** if arguments are equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="2bc11-392">Är inte lika med.</span><span class="sxs-lookup"><span data-stu-id="2bc11-392">Not equal to.</span></span> <span data-ttu-id="2bc11-393">Returnerar **SANT** om argumenten inte är lika, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-393">Returns **true** if arguments are not equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="2bc11-394">Större än.</span><span class="sxs-lookup"><span data-stu-id="2bc11-394">Greater Than.</span></span> <span data-ttu-id="2bc11-395">Returnerar **SANT** om det första argumentet är större än hello andra returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-395">Returns **true** if first argument is greater than hello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="2bc11-396">Större än eller lika med.</span><span class="sxs-lookup"><span data-stu-id="2bc11-396">Greater Than or Equal To.</span></span> <span data-ttu-id="2bc11-397">Returnerar **SANT** om det första argumentet är större än eller lika med toohello andra, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-397">Returns **true** if first argument is greater than or equal toohello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="2bc11-398">Mindre än.</span><span class="sxs-lookup"><span data-stu-id="2bc11-398">Less Than.</span></span> <span data-ttu-id="2bc11-399">Returnerar **SANT** om det första argumentet är mindre än hello andra, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-399">Returns **true** if first argument is less than hello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="2bc11-400">Mindre än eller lika med.</span><span class="sxs-lookup"><span data-stu-id="2bc11-400">Less Than or Equal To.</span></span> <span data-ttu-id="2bc11-401">Returnerar **SANT** om det första argumentet är mindre än eller lika med toohello andra en returtyp **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="2bc11-401">Returns **true** if first argument is less than or equal toohello second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="2bc11-402">Slå samman.</span><span class="sxs-lookup"><span data-stu-id="2bc11-402">Coalesce.</span></span> <span data-ttu-id="2bc11-403">Returnerar hello andra argumentet om hello första argumentet är en **Odefinierad** värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-403">Returns hello second argument if hello first argument is an **undefined** value.</span></span>|  
|<span data-ttu-id="2bc11-404">**Sträng**</span><span class="sxs-lookup"><span data-stu-id="2bc11-404">**String**</span></span>|<span data-ttu-id="2bc11-405">**&#124;&#124;**</span><span class="sxs-lookup"><span data-stu-id="2bc11-405">**&#124;&#124;**</span></span>|<span data-ttu-id="2bc11-406">Sammanfogning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-406">Concatenation.</span></span> <span data-ttu-id="2bc11-407">Returnerar en sammansättning av båda argumenten.</span><span class="sxs-lookup"><span data-stu-id="2bc11-407">Returns a concatenation of both arguments.</span></span>|  
  
 <span data-ttu-id="2bc11-408">**Ternär operator:**</span><span class="sxs-lookup"><span data-stu-id="2bc11-408">**Ternary operators:**</span></span>  
  
|<span data-ttu-id="2bc11-409">Ternär operator</span><span class="sxs-lookup"><span data-stu-id="2bc11-409">Ternary operator</span></span>|<span data-ttu-id="2bc11-410">?</span><span class="sxs-lookup"><span data-stu-id="2bc11-410">?</span></span>|<span data-ttu-id="2bc11-411">Returnerar hello andra argumentet om hello första argument evalueras för**SANT**; annars returnerar hello tredje argumentet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-411">Returns hello second argument if hello first argument evaluates too**true**; return hello third argument otherwise.</span></span>|  
|-|-|-|  
  
 <span data-ttu-id="2bc11-412">**Sorteringen av värden för jämförelse**</span><span class="sxs-lookup"><span data-stu-id="2bc11-412">**Ordering of values for comparison**</span></span>  
  
|<span data-ttu-id="2bc11-413">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2bc11-413">**Type**</span></span>|<span data-ttu-id="2bc11-414">**Värden ordning**</span><span class="sxs-lookup"><span data-stu-id="2bc11-414">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="2bc11-415">**Odefinierad**</span><span class="sxs-lookup"><span data-stu-id="2bc11-415">**Undefined**</span></span>|<span data-ttu-id="2bc11-416">Inte jämförbar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-416">Not comparable.</span></span>|  
|<span data-ttu-id="2bc11-417">**Null**</span><span class="sxs-lookup"><span data-stu-id="2bc11-417">**Null**</span></span>|<span data-ttu-id="2bc11-418">Enstaka värde: **null**</span><span class="sxs-lookup"><span data-stu-id="2bc11-418">Single value: **null**</span></span>|  
|<span data-ttu-id="2bc11-419">**Antal**</span><span class="sxs-lookup"><span data-stu-id="2bc11-419">**Number**</span></span>|<span data-ttu-id="2bc11-420">Naturliga tal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-420">Natural real number.</span></span><br /><br /> <span data-ttu-id="2bc11-421">Negativt oändligt värde är mindre än numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-421">Negative Infinity value is smaller than any other Number value.</span></span><br /><br /> <span data-ttu-id="2bc11-422">Positivt oändligt värde är större än värdet värde. **NaN** värde är inte jämförbar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-422">Positive Infinity value is larger than any other Number value.**NaN** value is not comparable.</span></span> <span data-ttu-id="2bc11-423">Jämföra med **NaN** leder **Odefinierad** värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-423">Comparing with **NaN** will result in **undefined** value.</span></span>|  
|<span data-ttu-id="2bc11-424">**Sträng**</span><span class="sxs-lookup"><span data-stu-id="2bc11-424">**String**</span></span>|<span data-ttu-id="2bc11-425">Lexicographical ordning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-425">Lexicographical order.</span></span>|  
|<span data-ttu-id="2bc11-426">**Matris**</span><span class="sxs-lookup"><span data-stu-id="2bc11-426">**Array**</span></span>|<span data-ttu-id="2bc11-427">Ingen ordning, men balanserad.</span><span class="sxs-lookup"><span data-stu-id="2bc11-427">No ordering, but equitable.</span></span>|  
|<span data-ttu-id="2bc11-428">**Objektet**</span><span class="sxs-lookup"><span data-stu-id="2bc11-428">**Object**</span></span>|<span data-ttu-id="2bc11-429">Ingen ordning, men balanserad.</span><span class="sxs-lookup"><span data-stu-id="2bc11-429">No ordering, but equitable.</span></span>|  
  
 <span data-ttu-id="2bc11-430">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-430">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-431">I Azure Cosmos DB kallas ofta inte hello typer av värden tills de faktiskt har hämtats från hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-431">In Azure Cosmos DB, hello types of values are often not known until they are actually retrieved from hello database.</span></span> <span data-ttu-id="2bc11-432">De flesta av hello operatörer har i ordning toosupport effektivt utförande av frågor, strikt krav.</span><span class="sxs-lookup"><span data-stu-id="2bc11-432">In order toosupport efficient execution of queries, most of hello operators have strict type requirements.</span></span> <span data-ttu-id="2bc11-433">Operatörer ensamt utför även inte implicita konverteringar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-433">Also operators by themselves do not perform implicit conversions.</span></span>  
  
 <span data-ttu-id="2bc11-434">Det innebär att en fråga som: Välj * från ROOT r var r.Age = 21 returneras endast dokument med egenskapen ålder lika toohello nummer 21.</span><span class="sxs-lookup"><span data-stu-id="2bc11-434">This means that a query like: SELECT * FROM ROOT r WHERE r.Age = 21 will only return documents with property Age equal toohello number 21.</span></span> <span data-ttu-id="2bc11-435">Dokument med egenskapen ålder lika toohello strängen ”21” eller hello strängen ”0021” kommer inte matchar som hello uttryck ”21” = 21 utvärderar tooundefined.</span><span class="sxs-lookup"><span data-stu-id="2bc11-435">Documents with property Age equal toohello string "21" or hello string "0021" will not match, as hello expression "21" = 21 evaluates tooundefined.</span></span> <span data-ttu-id="2bc11-436">Detta ger en bättre användning av index, eftersom hello sökning efter ett visst värde (d.v.s. number 21) är snabbare än att söka efter ett obestämt antal möjliga matchningar (dvs. antalet 21 eller strängar ”21”, ”021”, ”21.0”...).</span><span class="sxs-lookup"><span data-stu-id="2bc11-436">This allows for a better use of indexes, because hello lookup of a specific value (i.e. number 21) is faster than search for indefinite number of potential matches (i.e. number 21 or strings "21", "021", "21.0" …).</span></span> <span data-ttu-id="2bc11-437">Detta skiljer sig från hur JavaScript utvärderar operatorer på värden av olika typer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-437">This is different from how JavaScript evaluates operators on values of different types.</span></span>  
  
 <span data-ttu-id="2bc11-438">**Jämförelse av och likhetsfrågor för matriser och -objekt**</span><span class="sxs-lookup"><span data-stu-id="2bc11-438">**Arrays and objects equality and comparison**</span></span>  
  
 <span data-ttu-id="2bc11-439">Jämförelse av värden för matris eller ett objekt som använder range-operatorer (>, > =, <, < =) leder till odefinierad eftersom det inte finns inte ordning som har definierats för objektet eller matrisen värden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-439">Comparing of Array or Object values using range operators (>, >=, <, <=) will result in undefined as there is not order defined on Object or Array values.</span></span> <span data-ttu-id="2bc11-440">Men använder likhet/olikhet operatorer (=,! = <>) stöds och värden som ska jämföras strukturellt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-440">However using equality/inequality operators (=, !=, <>) is supported and values will be compared structurally.</span></span>  
  
 <span data-ttu-id="2bc11-441">Matriser är lika om båda matriser har samma antal element och elementen på matchar positioner också är lika.</span><span class="sxs-lookup"><span data-stu-id="2bc11-441">Arrays are equal if both arrays have same number of elements and elements at matching positions are also equal.</span></span> <span data-ttu-id="2bc11-442">Om att jämföra ett par element resulterar i odefinierat, hello resultatet av matrisen jämförelse är odefinierad.</span><span class="sxs-lookup"><span data-stu-id="2bc11-442">If comparing any pair of elements results in undefined, hello result of array comparison is undefined.</span></span>  
  
 <span data-ttu-id="2bc11-443">Objekt är lika om både objekt har samma egenskaper som definierats och värden av matchande egenskaper också är lika.</span><span class="sxs-lookup"><span data-stu-id="2bc11-443">Objects are equal if both objects have same properties defined, and if values of matching properties are also equal.</span></span> <span data-ttu-id="2bc11-444">Om att jämföra ett par egenskapsvärden resulterar i odefinierat, hello resultatet av objektet jämförelse är odefinierad.</span><span class="sxs-lookup"><span data-stu-id="2bc11-444">If comparing any pair of property values results in undefined, hello result of object comparison is undefined.</span></span>  
  
##  <span data-ttu-id="2bc11-445"><a name="bk_constants"></a>Konstanter</span><span class="sxs-lookup"><span data-stu-id="2bc11-445"><a name="bk_constants"></a> Constants</span></span>  
 <span data-ttu-id="2bc11-446">En konstant, även kallat ett litteralvärde eller ett skalärt värde är en symbol som representerar ett specifikt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-446">A constant, also known as a literal or a scalar value, is a symbol that represents a specific data value.</span></span> <span data-ttu-id="2bc11-447">hello-format för en konstant beror på hello datatypen för hello-värdet som representerar.</span><span class="sxs-lookup"><span data-stu-id="2bc11-447">hello format of a constant depends on hello data type of hello value it represents.</span></span>  
  
 <span data-ttu-id="2bc11-448">**Skalära datatyper som stöds:**</span><span class="sxs-lookup"><span data-stu-id="2bc11-448">**Supported scalar data types:**</span></span>  
  
|<span data-ttu-id="2bc11-449">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2bc11-449">**Type**</span></span>|<span data-ttu-id="2bc11-450">**Värden ordning**</span><span class="sxs-lookup"><span data-stu-id="2bc11-450">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="2bc11-451">**Odefinierad**</span><span class="sxs-lookup"><span data-stu-id="2bc11-451">**Undefined**</span></span>|<span data-ttu-id="2bc11-452">Enstaka värde: **Odefinierad**</span><span class="sxs-lookup"><span data-stu-id="2bc11-452">Single value: **undefined**</span></span>|  
|<span data-ttu-id="2bc11-453">**Null**</span><span class="sxs-lookup"><span data-stu-id="2bc11-453">**Null**</span></span>|<span data-ttu-id="2bc11-454">Enstaka värde: **null**</span><span class="sxs-lookup"><span data-stu-id="2bc11-454">Single value: **null**</span></span>|  
|<span data-ttu-id="2bc11-455">**Booleskt värde**</span><span class="sxs-lookup"><span data-stu-id="2bc11-455">**Boolean**</span></span>|<span data-ttu-id="2bc11-456">Värden: **FALSKT**, **SANT**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-456">Values: **false**, **true**.</span></span>|  
|<span data-ttu-id="2bc11-457">**Antal**</span><span class="sxs-lookup"><span data-stu-id="2bc11-457">**Number**</span></span>|<span data-ttu-id="2bc11-458">En dubbel precision flyttal, IEEE-754 som standard.</span><span class="sxs-lookup"><span data-stu-id="2bc11-458">A double-precision floating-point number, IEEE 754 standard.</span></span>|  
|<span data-ttu-id="2bc11-459">**Sträng**</span><span class="sxs-lookup"><span data-stu-id="2bc11-459">**String**</span></span>|<span data-ttu-id="2bc11-460">En sekvens med noll eller flera Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-460">A sequence of zero or more Unicode characters.</span></span> <span data-ttu-id="2bc11-461">Strängar måste stå inom enkla eller dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-461">Strings must be enclosed in single or double quotes.</span></span>|  
|<span data-ttu-id="2bc11-462">**Matris**</span><span class="sxs-lookup"><span data-stu-id="2bc11-462">**Array**</span></span>|<span data-ttu-id="2bc11-463">En sekvens med noll eller flera element.</span><span class="sxs-lookup"><span data-stu-id="2bc11-463">A sequence of zero or more elements.</span></span> <span data-ttu-id="2bc11-464">Varje element kan vara ett värde med en skalär datatypen, utom Undefined.</span><span class="sxs-lookup"><span data-stu-id="2bc11-464">Each element can be a value of any scalar data type, except Undefined.</span></span>|  
|<span data-ttu-id="2bc11-465">**Objektet**</span><span class="sxs-lookup"><span data-stu-id="2bc11-465">**Object**</span></span>|<span data-ttu-id="2bc11-466">En osorterad uppsättning noll eller flera namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="2bc11-466">An unordered set of zero or more name/value pairs.</span></span> <span data-ttu-id="2bc11-467">Namnet är en Unicode-sträng, värdet kan vara av valfri skalära datatyp, utom **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-467">Name is a Unicode string, value can be of any scalar data type, except **Undefined**.</span></span>|  
  
 <span data-ttu-id="2bc11-468">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-468">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="2bc11-469">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-469">**Arguments**</span></span>  
  
1.  `<undefined_constant>; undefined`  
  
     <span data-ttu-id="2bc11-470">Representerar odefinierat värde av typen odefinierad.</span><span class="sxs-lookup"><span data-stu-id="2bc11-470">Represents undefined value of type Undefined.</span></span>  
  
2.  `<null_constant>; null`  
  
     <span data-ttu-id="2bc11-471">Representerar **null** värde av typen **Null**.</span><span class="sxs-lookup"><span data-stu-id="2bc11-471">Represents **null** value of type **Null**.</span></span>  
  
3.  `<boolean_constant>`  
  
     <span data-ttu-id="2bc11-472">Representerar konstanter av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="2bc11-472">Represents constant of type Boolean.</span></span>  
  
4.  `false`  
  
     <span data-ttu-id="2bc11-473">Representerar **FALSKT** värde av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="2bc11-473">Represents **false** value of type Boolean.</span></span>  
  
5.  `true`  
  
     <span data-ttu-id="2bc11-474">Representerar **SANT** värde av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="2bc11-474">Represents **true** value of type Boolean.</span></span>  
  
6.  `<number_constant>`  
  
     <span data-ttu-id="2bc11-475">Representerar en konstant.</span><span class="sxs-lookup"><span data-stu-id="2bc11-475">Represents a constant.</span></span>  
  
7.  `decimal_literal`  
  
     <span data-ttu-id="2bc11-476">Decimal literaler är nummer som representeras med decimalform eller matematisk notation.</span><span class="sxs-lookup"><span data-stu-id="2bc11-476">Decimal literals are numbers represented using either decimal notation, or scientific notation.</span></span>  
  
8.  `hexadecimal_literal`  
  
     <span data-ttu-id="2bc11-477">Hexadecimala strängar är nummer som representeras med prefixet '0 x' följt av en eller flera hexadecimala siffror.</span><span class="sxs-lookup"><span data-stu-id="2bc11-477">Hexadecimal literals are numbers represented using prefix '0x' followed by one or more hexadecimal digits.</span></span>  
  
9. `<string_constant>`  
  
     <span data-ttu-id="2bc11-478">Representerar en konstant av typen String.</span><span class="sxs-lookup"><span data-stu-id="2bc11-478">Represents a constant of type String.</span></span>  
  
10. `string _literal`  
  
     <span data-ttu-id="2bc11-479">Stränglitteraler är Unicode-strängar som representeras av en följd av noll eller flera Unicode-tecken eller escape-sekvenser.</span><span class="sxs-lookup"><span data-stu-id="2bc11-479">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="2bc11-480">Stränglitteraler omges av enkla citattecken (apostrof: ') eller dubbla citattecken (citattecken ”:).</span><span class="sxs-lookup"><span data-stu-id="2bc11-480">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span>  
  
 <span data-ttu-id="2bc11-481">Följande escape-sekvenser tillåts:</span><span class="sxs-lookup"><span data-stu-id="2bc11-481">Following escape sequences are allowed:</span></span>  
  
|<span data-ttu-id="2bc11-482">**Escape-sekvens**</span><span class="sxs-lookup"><span data-stu-id="2bc11-482">**Escape sequence**</span></span>|<span data-ttu-id="2bc11-483">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="2bc11-483">**Description**</span></span>|<span data-ttu-id="2bc11-484">**Unicode-tecken**</span><span class="sxs-lookup"><span data-stu-id="2bc11-484">**Unicode character**</span></span>|  
|-|-|-|  
|<span data-ttu-id="2bc11-485">\\'</span><span class="sxs-lookup"><span data-stu-id="2bc11-485">\\'</span></span>|<span data-ttu-id="2bc11-486">apostrof (')</span><span class="sxs-lookup"><span data-stu-id="2bc11-486">apostrophe (')</span></span>|<span data-ttu-id="2bc11-487">U + 0027</span><span class="sxs-lookup"><span data-stu-id="2bc11-487">U+0027</span></span>|  
|<span data-ttu-id="2bc11-488">\\"</span><span class="sxs-lookup"><span data-stu-id="2bc11-488">\\"</span></span>|<span data-ttu-id="2bc11-489">citattecken (”)</span><span class="sxs-lookup"><span data-stu-id="2bc11-489">quotation mark (")</span></span>|<span data-ttu-id="2bc11-490">U + 0022</span><span class="sxs-lookup"><span data-stu-id="2bc11-490">U+0022</span></span>|  
|\\\|<span data-ttu-id="2bc11-491">omvänd solidus (\\)</span><span class="sxs-lookup"><span data-stu-id="2bc11-491">reverse solidus (\\)</span></span>|<span data-ttu-id="2bc11-492">U + 005C</span><span class="sxs-lookup"><span data-stu-id="2bc11-492">U+005C</span></span>|  
|\\/|<span data-ttu-id="2bc11-493">solidus (/)</span><span class="sxs-lookup"><span data-stu-id="2bc11-493">solidus (/)</span></span>|<span data-ttu-id="2bc11-494">U + 002F</span><span class="sxs-lookup"><span data-stu-id="2bc11-494">U+002F</span></span>|  
|<span data-ttu-id="2bc11-495">\b</span><span class="sxs-lookup"><span data-stu-id="2bc11-495">\b</span></span>|<span data-ttu-id="2bc11-496">BACKSTEG</span><span class="sxs-lookup"><span data-stu-id="2bc11-496">backspace</span></span>|<span data-ttu-id="2bc11-497">U + 0008</span><span class="sxs-lookup"><span data-stu-id="2bc11-497">U+0008</span></span>|  
|<span data-ttu-id="2bc11-498">\f</span><span class="sxs-lookup"><span data-stu-id="2bc11-498">\f</span></span>|<span data-ttu-id="2bc11-499">formuläret feed</span><span class="sxs-lookup"><span data-stu-id="2bc11-499">form feed</span></span>|<span data-ttu-id="2bc11-500">U + 000C</span><span class="sxs-lookup"><span data-stu-id="2bc11-500">U+000C</span></span>|  
|\n|<span data-ttu-id="2bc11-501">radmatning</span><span class="sxs-lookup"><span data-stu-id="2bc11-501">line feed</span></span>|<span data-ttu-id="2bc11-502">U + 000A</span><span class="sxs-lookup"><span data-stu-id="2bc11-502">U+000A</span></span>|  
|<span data-ttu-id="2bc11-503">\r</span><span class="sxs-lookup"><span data-stu-id="2bc11-503">\r</span></span>|<span data-ttu-id="2bc11-504">vagnretur</span><span class="sxs-lookup"><span data-stu-id="2bc11-504">carriage return</span></span>|<span data-ttu-id="2bc11-505">U + 000D</span><span class="sxs-lookup"><span data-stu-id="2bc11-505">U+000D</span></span>|  
|<span data-ttu-id="2bc11-506">\t</span><span class="sxs-lookup"><span data-stu-id="2bc11-506">\t</span></span>|<span data-ttu-id="2bc11-507">fliken</span><span class="sxs-lookup"><span data-stu-id="2bc11-507">tab</span></span>|<span data-ttu-id="2bc11-508">U + 0009</span><span class="sxs-lookup"><span data-stu-id="2bc11-508">U+0009</span></span>|  
|<span data-ttu-id="2bc11-509">\uXXXX</span><span class="sxs-lookup"><span data-stu-id="2bc11-509">\uXXXX</span></span>|<span data-ttu-id="2bc11-510">En Unicode-tecken som definieras av 4 hexadecimala siffror.</span><span class="sxs-lookup"><span data-stu-id="2bc11-510">A Unicode character defined by 4 hexadecimal digits.</span></span>|<span data-ttu-id="2bc11-511">U + XXXX</span><span class="sxs-lookup"><span data-stu-id="2bc11-511">U+XXXX</span></span>|  
  
##  <span data-ttu-id="2bc11-512"><a name="bk_query_perf_guidelines"></a>Riktlinjer för frågan prestanda</span><span class="sxs-lookup"><span data-stu-id="2bc11-512"><a name="bk_query_perf_guidelines"></a> Query performance guidelines</span></span>  
 <span data-ttu-id="2bc11-513">Det bör använda filter som kan hanteras via ett eller flera index för en fråga toobe körs effektivt för en stor samling.</span><span class="sxs-lookup"><span data-stu-id="2bc11-513">In order for a query toobe executed efficiently for a large collection, it should use filters which can be served through one or more indexes.</span></span>  
  
 <span data-ttu-id="2bc11-514">följande filter hello beaktas för index sökning:</span><span class="sxs-lookup"><span data-stu-id="2bc11-514">hello following filters will be considered for index lookup:</span></span>  
  
-   <span data-ttu-id="2bc11-515">Använd Likhetsoperatorn (=) med ett sökvägsuttryck för dokumentet och en konstant.</span><span class="sxs-lookup"><span data-stu-id="2bc11-515">Use equality operator ( = ) with a document path expression and a constant.</span></span>  
  
-   <span data-ttu-id="2bc11-516">Använd range-operatorer (<, \<=, >, > =) med ett sökvägsuttryck för dokumentet och antalet konstanter.</span><span class="sxs-lookup"><span data-stu-id="2bc11-516">Use range operators (<, \<=, >, >=) with a document path expression and number constants.</span></span>  
  
-   <span data-ttu-id="2bc11-517">Dokumentet sökvägsuttryck står för ett uttryck som identifierar en konstant sökväg i hello dokument från hello refererar till databasen samling.</span><span class="sxs-lookup"><span data-stu-id="2bc11-517">Document path expression stands for any expression which identifies a constant path in hello documents from hello referenced database collection.</span></span>  
  
 <span data-ttu-id="2bc11-518">**Dokumentet sökvägsuttryck**</span><span class="sxs-lookup"><span data-stu-id="2bc11-518">**Document path expression**</span></span>  
  
 <span data-ttu-id="2bc11-519">Dokumentet sökvägsuttryck är uttryck som en sökväg till matrisen egenskapen eller indexeraren bedömare över ett dokument som kommer från databasen samling dokument.</span><span class="sxs-lookup"><span data-stu-id="2bc11-519">Document path expressions are expressions that a path of property or array indexer assessors over a document coming from database collection documents.</span></span> <span data-ttu-id="2bc11-520">Den här sökvägen kan vara används tooidentify hello platsen för värden som refereras i ett filter direkt i hello dokument i hello databasen samling.</span><span class="sxs-lookup"><span data-stu-id="2bc11-520">This path can be used tooidentify hello location of values referenced in a filter directly within hello documents in hello database collection.</span></span>  
  
 <span data-ttu-id="2bc11-521">För ett uttryck toobe anses vara ett dokument sökvägsuttryck, bör det:</span><span class="sxs-lookup"><span data-stu-id="2bc11-521">For an expression toobe considered a document path expression, it should:</span></span>  
  
1.  <span data-ttu-id="2bc11-522">Referens hello samling root direkt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-522">Reference hello collection root directly.</span></span>  
  
2.  <span data-ttu-id="2bc11-523">Indexerare för referens-egenskapen eller konstant matris för vissa dokument sökvägsuttryck</span><span class="sxs-lookup"><span data-stu-id="2bc11-523">Reference property or constant array indexer of some document path expression</span></span>  
  
3.  <span data-ttu-id="2bc11-524">Referera till ett alias som representerar vissa dokument sökvägsuttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-524">Reference an alias, which represents some document path expression.</span></span>  
  
     <span data-ttu-id="2bc11-525">**Syntaxen konventioner**</span><span class="sxs-lookup"><span data-stu-id="2bc11-525">**Syntax conventions**</span></span>  
  
     <span data-ttu-id="2bc11-526">hello i den följande tabellen beskrivs hello konventioner toodescribe syntax i hello frågespråket i DocumentDB API-referens.</span><span class="sxs-lookup"><span data-stu-id="2bc11-526">hello following table describes hello conventions used toodescribe syntax in hello DocumentDB API Query Language reference.</span></span>  
  
    |<span data-ttu-id="2bc11-527">**Konventionen**</span><span class="sxs-lookup"><span data-stu-id="2bc11-527">**Convention**</span></span>|<span data-ttu-id="2bc11-528">**Används för**</span><span class="sxs-lookup"><span data-stu-id="2bc11-528">**Used for**</span></span>|  
    |-|-|    
    |<span data-ttu-id="2bc11-529">VERSALER</span><span class="sxs-lookup"><span data-stu-id="2bc11-529">UPPERCASE</span></span>|<span data-ttu-id="2bc11-530">Skiftlägeskänsliga nyckelord.</span><span class="sxs-lookup"><span data-stu-id="2bc11-530">Case-insensitive keywords.</span></span>|  
    |<span data-ttu-id="2bc11-531">gemener</span><span class="sxs-lookup"><span data-stu-id="2bc11-531">lowercase</span></span>|<span data-ttu-id="2bc11-532">Skiftlägeskänsligt nyckelord.</span><span class="sxs-lookup"><span data-stu-id="2bc11-532">Case-sensitive keywords.</span></span>|  
    |<span data-ttu-id="2bc11-533">\<nonterminal ></span><span class="sxs-lookup"><span data-stu-id="2bc11-533">\<nonterminal></span></span>|<span data-ttu-id="2bc11-534">Öppen, som har definierats separat.</span><span class="sxs-lookup"><span data-stu-id="2bc11-534">Nonterminal, defined separately.</span></span>|  
    |<span data-ttu-id="2bc11-535">\<nonterminal >:: =</span><span class="sxs-lookup"><span data-stu-id="2bc11-535">\<nonterminal> ::=</span></span>|<span data-ttu-id="2bc11-536">Syntax för definition av hello nonterminal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-536">Syntax definition of hello nonterminal.</span></span>|  
    |<span data-ttu-id="2bc11-537">other_terminal</span><span class="sxs-lookup"><span data-stu-id="2bc11-537">other_terminal</span></span>|<span data-ttu-id="2bc11-538">Terminal (token), som beskrivs i detalj i ord.</span><span class="sxs-lookup"><span data-stu-id="2bc11-538">Terminal (token), described in detail in words.</span></span>|  
    |<span data-ttu-id="2bc11-539">Identifierare</span><span class="sxs-lookup"><span data-stu-id="2bc11-539">identifier</span></span>|<span data-ttu-id="2bc11-540">Identifierare.</span><span class="sxs-lookup"><span data-stu-id="2bc11-540">Identifier.</span></span> <span data-ttu-id="2bc11-541">Gör följande endast tecken: a-z A-Z 0-9 _First tecknet får inte vara en siffra.</span><span class="sxs-lookup"><span data-stu-id="2bc11-541">Allows following characters only: a-z A-Z 0-9 _First character cannot be a digit.</span></span>|  
    |<span data-ttu-id="2bc11-542">”sträng”</span><span class="sxs-lookup"><span data-stu-id="2bc11-542">"string"</span></span>|<span data-ttu-id="2bc11-543">Sträng inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-543">Quoted string.</span></span> <span data-ttu-id="2bc11-544">Gör att en giltig sträng.</span><span class="sxs-lookup"><span data-stu-id="2bc11-544">Allows any valid string.</span></span> <span data-ttu-id="2bc11-545">Se beskrivning av en string_literal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-545">See description of a string_literal.</span></span>|  
    |<span data-ttu-id="2bc11-546">'symbolen'</span><span class="sxs-lookup"><span data-stu-id="2bc11-546">'symbol'</span></span>|<span data-ttu-id="2bc11-547">Literalen symbol som ingår i hello syntax.</span><span class="sxs-lookup"><span data-stu-id="2bc11-547">Literal symbol which is part of hello syntax.</span></span>|  
    |<span data-ttu-id="2bc11-548">&#124; (lodrätt streck)</span><span class="sxs-lookup"><span data-stu-id="2bc11-548">&#124; (vertical bar)</span></span>|<span data-ttu-id="2bc11-549">Alternativ för syntax objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-549">Alternatives for syntax items.</span></span> <span data-ttu-id="2bc11-550">Du kan använda endast en av de angivna hello-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-550">You can use only one of hello items specified.</span></span>|  
    |<span data-ttu-id="2bc11-551">[] /(brackets)</span><span class="sxs-lookup"><span data-stu-id="2bc11-551">[ ] /(brackets)</span></span>|<span data-ttu-id="2bc11-552">Hakparenteser innefatta en eller flera valfria objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-552">Brackets enclose one or more optional items.</span></span>|  
    |<span data-ttu-id="2bc11-553">[,.. .n]</span><span class="sxs-lookup"><span data-stu-id="2bc11-553">[ ,...n ]</span></span>|<span data-ttu-id="2bc11-554">Anger hello föregående artikel kan vara upprepade n är antalet gånger.</span><span class="sxs-lookup"><span data-stu-id="2bc11-554">Indicates hello preceding item can be repeated n number of times.</span></span> <span data-ttu-id="2bc11-555">hello förekomster avgränsas med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-555">hello occurrences are separated by commas.</span></span>|  
    |<span data-ttu-id="2bc11-556">[.. .n]</span><span class="sxs-lookup"><span data-stu-id="2bc11-556">[ ...n ]</span></span>|<span data-ttu-id="2bc11-557">Anger hello föregående artikel kan vara upprepade n är antalet gånger.</span><span class="sxs-lookup"><span data-stu-id="2bc11-557">Indicates hello preceding item can be repeated n number of times.</span></span> <span data-ttu-id="2bc11-558">hello förekomster avgränsas med blanksteg.</span><span class="sxs-lookup"><span data-stu-id="2bc11-558">hello occurrences are separated by blanks.</span></span>|  
  
##  <span data-ttu-id="2bc11-559"><a name="bk_built_in_functions"></a>Inbyggda funktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-559"><a name="bk_built_in_functions"></a> Built-in functions</span></span>  
 <span data-ttu-id="2bc11-560">Azure Cosmos-DB innehåller många inbyggda SQL-funktioner.</span><span class="sxs-lookup"><span data-stu-id="2bc11-560">Azure Cosmos DB provides many built-in SQL functions.</span></span> <span data-ttu-id="2bc11-561">hello kategorier av inbyggda funktioner i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-561">hello categories of built-in functions are listed below.</span></span>  
  
|<span data-ttu-id="2bc11-562">Funktionen</span><span class="sxs-lookup"><span data-stu-id="2bc11-562">Function</span></span>|<span data-ttu-id="2bc11-563">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2bc11-563">Description</span></span>|  
|--------------|-----------------|  
|[<span data-ttu-id="2bc11-564">Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-564">Mathematical functions</span></span>](#bk_mathematical_functions)|<span data-ttu-id="2bc11-565">hello matematiska funktioner varje utför en beräkning, vanligtvis baserat på värden som har angetts som argument och returnerar ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-565">hello mathematical functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>|  
|[<span data-ttu-id="2bc11-566">Ange kontrollerar funktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-566">Type checking functions</span></span>](#bk_type_checking_functions)|<span data-ttu-id="2bc11-567">hello typ kontrollerar funktioner kan du toocheck hello typ av ett uttryck i SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="2bc11-567">hello type checking functions allow you toocheck hello type of an expression within SQL queries.</span></span>|  
|[<span data-ttu-id="2bc11-568">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-568">String functions</span></span>](#bk_string_functions)|<span data-ttu-id="2bc11-569">hello strängfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-569">hello string functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>|  
|[<span data-ttu-id="2bc11-570">Matrisfunktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-570">Array functions</span></span>](#bk_array_functions)|<span data-ttu-id="2bc11-571">hello matrisfunktioner kan du utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-571">hello array functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span>|  
|[<span data-ttu-id="2bc11-572">Spatial funktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-572">Spatial functions</span></span>](#bk_spatial_functions)|<span data-ttu-id="2bc11-573">hello spatial funktioner utföra en åtgärd på ett indatavärde Rumsobjektet och returnera ett numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-573">hello spatial functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>|  
  
###  <span data-ttu-id="2bc11-574"><a name="bk_mathematical_functions"></a>Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-574"><a name="bk_mathematical_functions"></a> Mathematical functions</span></span>  
 <span data-ttu-id="2bc11-575">hello utför följande funktioner en beräkning, vanligtvis baserat på värden som har angetts som argument och returnerar ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-575">hello following functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="2bc11-576">ABS</span><span class="sxs-lookup"><span data-stu-id="2bc11-576">ABS</span></span>](#bk_abs)|[<span data-ttu-id="2bc11-577">ARCCOS</span><span class="sxs-lookup"><span data-stu-id="2bc11-577">ACOS</span></span>](#bk_acos)|[<span data-ttu-id="2bc11-578">ARCSIN</span><span class="sxs-lookup"><span data-stu-id="2bc11-578">ASIN</span></span>](#bk_asin)|  
|[<span data-ttu-id="2bc11-579">ARCTAN</span><span class="sxs-lookup"><span data-stu-id="2bc11-579">ATAN</span></span>](#bk_atan)|[<span data-ttu-id="2bc11-580">ATN2</span><span class="sxs-lookup"><span data-stu-id="2bc11-580">ATN2</span></span>](#bk_atn2)|[<span data-ttu-id="2bc11-581">TAK</span><span class="sxs-lookup"><span data-stu-id="2bc11-581">CEILING</span></span>](#bk_ceiling)|  
|[<span data-ttu-id="2bc11-582">COS</span><span class="sxs-lookup"><span data-stu-id="2bc11-582">COS</span></span>](#bk_cos)|[<span data-ttu-id="2bc11-583">BET</span><span class="sxs-lookup"><span data-stu-id="2bc11-583">COT</span></span>](#bk_cot)|[<span data-ttu-id="2bc11-584">GRADER</span><span class="sxs-lookup"><span data-stu-id="2bc11-584">DEGREES</span></span>](#bk_degrees)|  
|[<span data-ttu-id="2bc11-585">EXP</span><span class="sxs-lookup"><span data-stu-id="2bc11-585">EXP</span></span>](#bk_exp)|[<span data-ttu-id="2bc11-586">VÅNING</span><span class="sxs-lookup"><span data-stu-id="2bc11-586">FLOOR</span></span>](#bk_floor)|[<span data-ttu-id="2bc11-587">LOGG</span><span class="sxs-lookup"><span data-stu-id="2bc11-587">LOG</span></span>](#bk_log)|  
|[<span data-ttu-id="2bc11-588">LOG10</span><span class="sxs-lookup"><span data-stu-id="2bc11-588">LOG10</span></span>](#bk_log10)|[<span data-ttu-id="2bc11-589">PI</span><span class="sxs-lookup"><span data-stu-id="2bc11-589">PI</span></span>](#bk_pi)|[<span data-ttu-id="2bc11-590">POWER</span><span class="sxs-lookup"><span data-stu-id="2bc11-590">POWER</span></span>](#bk_power)|  
|[<span data-ttu-id="2bc11-591">RADIANER</span><span class="sxs-lookup"><span data-stu-id="2bc11-591">RADIANS</span></span>](#bk_radians)|[<span data-ttu-id="2bc11-592">AVRUNDA</span><span class="sxs-lookup"><span data-stu-id="2bc11-592">ROUND</span></span>](#bk_round)|[<span data-ttu-id="2bc11-593">SIN</span><span class="sxs-lookup"><span data-stu-id="2bc11-593">SIN</span></span>](#bk_sin)|  
|[<span data-ttu-id="2bc11-594">ROT</span><span class="sxs-lookup"><span data-stu-id="2bc11-594">SQRT</span></span>](#bk_sqrt)|[<span data-ttu-id="2bc11-595">RUTA</span><span class="sxs-lookup"><span data-stu-id="2bc11-595">SQUARE</span></span>](#bk_square)|[<span data-ttu-id="2bc11-596">LOGGA</span><span class="sxs-lookup"><span data-stu-id="2bc11-596">SIGN</span></span>](#bk_sign)|  
|[<span data-ttu-id="2bc11-597">TAN</span><span class="sxs-lookup"><span data-stu-id="2bc11-597">TAN</span></span>](#bk_tan)|[<span data-ttu-id="2bc11-598">AVKORTA</span><span class="sxs-lookup"><span data-stu-id="2bc11-598">TRUNC</span></span>](#bk_trunc)||  
  
####  <span data-ttu-id="2bc11-599"><a name="bk_abs"></a>ABS</span><span class="sxs-lookup"><span data-stu-id="2bc11-599"><a name="bk_abs"></a> ABS</span></span>  
 <span data-ttu-id="2bc11-600">Returnerar hello absolut (positivt) värdet för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-600">Returns hello absolute (positive) value of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-601">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-601">**Syntax**</span></span>  
  
```  
ABS (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-602">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-602">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-603">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-603">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-604">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-604">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-605">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-605">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-606">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-606">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-607">hello visar följande exempel hello resultatet av funktionen hello ABS på tre olika nummer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-607">hello following example shows hello results of using hello ABS function on three different numbers.</span></span>  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 <span data-ttu-id="2bc11-608">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-608">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <span data-ttu-id="2bc11-609"><a name="bk_acos"></a>ARCCOS</span><span class="sxs-lookup"><span data-stu-id="2bc11-609"><a name="bk_acos"></a> ACOS</span></span>  
 <span data-ttu-id="2bc11-610">Returnerar hello vinkel angivna i radianer, vars cosinus är hello numeriska uttrycket; kallas även cosinus.</span><span class="sxs-lookup"><span data-stu-id="2bc11-610">Returns hello angle, in radians, whose cosine is hello specified numeric expression; also called arccosine.</span></span>  
  
 <span data-ttu-id="2bc11-611">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-611">**Syntax**</span></span>  
  
```  
ACOS(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-612">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-612">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-613">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-613">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-614">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-614">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-615">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-615">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-616">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-616">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-617">hello returneras följande exempel hello ARCCOS 1.</span><span class="sxs-lookup"><span data-stu-id="2bc11-617">hello following example returns hello ACOS of -1.</span></span>  
  
```  
SELECT ACOS(-1)  
```  
  
 <span data-ttu-id="2bc11-618">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-618">Here is hello result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="2bc11-619"><a name="bk_asin"></a>ARCSIN</span><span class="sxs-lookup"><span data-stu-id="2bc11-619"><a name="bk_asin"></a> ASIN</span></span>  
 <span data-ttu-id="2bc11-620">Returnerar hello vinkel angivna i radianer, vars sinus är hello numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-620">Returns hello angle, in radians, whose sine is hello specified numeric expression.</span></span> <span data-ttu-id="2bc11-621">Detta kallas också arcsinus.</span><span class="sxs-lookup"><span data-stu-id="2bc11-621">This is also called arcsine.</span></span>  
  
 <span data-ttu-id="2bc11-622">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-622">**Syntax**</span></span>  
  
```  
ASIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-623">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-623">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-624">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-624">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-625">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-625">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-626">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-626">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-627">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-627">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-628">hello returneras följande exempel hello ARCSIN 1.</span><span class="sxs-lookup"><span data-stu-id="2bc11-628">hello following example returns hello ASIN of -1.</span></span>  
  
```  
SELECT ASIN(-1)  
```  
  
 <span data-ttu-id="2bc11-629">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-629">Here is hello result set.</span></span>  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <span data-ttu-id="2bc11-630"><a name="bk_atan"></a>ARCTAN</span><span class="sxs-lookup"><span data-stu-id="2bc11-630"><a name="bk_atan"></a> ATAN</span></span>  
 <span data-ttu-id="2bc11-631">Returnerar hello vinkel angivna i radianer, vars tangens är hello numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-631">Returns hello angle, in radians, whose tangent is hello specified numeric expression.</span></span> <span data-ttu-id="2bc11-632">Detta kallas också tangens.</span><span class="sxs-lookup"><span data-stu-id="2bc11-632">This is also called arctangent.</span></span>  
  
 <span data-ttu-id="2bc11-633">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-633">**Syntax**</span></span>  
  
```  
ATAN(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-634">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-634">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-635">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-635">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-636">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-636">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-637">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-637">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-638">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-638">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-639">följande exempel returnerar hello ARCTAN av hello hello angivet värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-639">hello following example returns hello ATAN of hello specified value.</span></span>  
  
```  
SELECT ATAN(-45.01)  
```  
  
 <span data-ttu-id="2bc11-640">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-640">Here is hello result set.</span></span>  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <span data-ttu-id="2bc11-641"><a name="bk_atn2"></a>ATN2</span><span class="sxs-lookup"><span data-stu-id="2bc11-641"><a name="bk_atn2"></a> ATN2</span></span>  
 <span data-ttu-id="2bc11-642">Returnerar hello huvudvärdet hello arcus tangens för y / x, uttryckt i radianer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-642">Returns hello principal value of hello arc tangent of y/x, expressed in radians.</span></span>  
  
 <span data-ttu-id="2bc11-643">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-643">**Syntax**</span></span>  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-644">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-644">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-645">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-645">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-646">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-646">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-647">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-647">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-648">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-648">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-649">hello följande exempel beräknas hello ATN2 för hello angetts x och y komponenter.</span><span class="sxs-lookup"><span data-stu-id="2bc11-649">hello following example calculates hello ATN2 for hello specified x and y components.</span></span>  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 <span data-ttu-id="2bc11-650">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-650">Here is hello result set.</span></span>  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <span data-ttu-id="2bc11-651"><a name="bk_ceiling"></a>TAK</span><span class="sxs-lookup"><span data-stu-id="2bc11-651"><a name="bk_ceiling"></a> CEILING</span></span>  
 <span data-ttu-id="2bc11-652">Returnerar hello minsta heltal som är större än eller lika med för att hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-652">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-653">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-653">**Syntax**</span></span>  
  
```  
CEILING (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-654">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-654">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-655">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-655">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-656">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-656">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-657">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-657">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-658">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-658">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-659">hello som följande exempel visar positivt numeriskt negativt, och noll värden med hello TAKET funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-659">hello following example shows positive numeric, negative, and zero values with hello CEILING function.</span></span>  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 <span data-ttu-id="2bc11-660">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-660">Here is hello result set.</span></span>  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <span data-ttu-id="2bc11-661"><a name="bk_cos"></a>COS</span><span class="sxs-lookup"><span data-stu-id="2bc11-661"><a name="bk_cos"></a> COS</span></span>  
 <span data-ttu-id="2bc11-662">Returnerar hello trigonometriska värden cosinus för hello angiven vinkel i radianer, i hello angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-662">Returns hello trigonometric cosine of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="2bc11-663">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-663">**Syntax**</span></span>  
  
```  
COS(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-664">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-664">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-665">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-665">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-666">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-666">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-667">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-667">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-668">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-668">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-669">följande exempel hello beräknar hello COS på hello vinkeln som anges.</span><span class="sxs-lookup"><span data-stu-id="2bc11-669">hello following example calculates hello COS of hello specified angle.</span></span>  
  
```  
SELECT COS(14.78)  
```  
  
 <span data-ttu-id="2bc11-670">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-670">Here is hello result set.</span></span>  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <span data-ttu-id="2bc11-671"><a name="bk_cot"></a>BET</span><span class="sxs-lookup"><span data-stu-id="2bc11-671"><a name="bk_cot"></a> COT</span></span>  
 <span data-ttu-id="2bc11-672">Returnerar hello trigonometriska värden cotangens för hello angiven vinkel i radianer, i hello anges numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-672">Returns hello trigonometric cotangent of hello specified angle, in radians, in hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-673">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-673">**Syntax**</span></span>  
  
```  
COT(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-674">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-674">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-675">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-675">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-676">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-676">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-677">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-677">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-678">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-678">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-679">följande exempel hello beräknar hello bet för hello angivna vinkeln.</span><span class="sxs-lookup"><span data-stu-id="2bc11-679">hello following example calculates hello COT of hello specified angle.</span></span>  
  
```  
SELECT COT(124.1332)  
```  
  
 <span data-ttu-id="2bc11-680">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-680">Here is hello result set.</span></span>  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <span data-ttu-id="2bc11-681"><a name="bk_degrees"></a>GRADER</span><span class="sxs-lookup"><span data-stu-id="2bc11-681"><a name="bk_degrees"></a> DEGREES</span></span>  
 <span data-ttu-id="2bc11-682">Returnerar hello motsvarande vinkel i grader för en vinkel angiven i radianer.</span><span class="sxs-lookup"><span data-stu-id="2bc11-682">Returns hello corresponding angle in degrees for an angle specified in radians.</span></span>  
  
 <span data-ttu-id="2bc11-683">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-683">**Syntax**</span></span>  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-684">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-684">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-685">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-685">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-686">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-686">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-687">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-687">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-688">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-688">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-689">hello returnerar följande exempel hello antal grader i vinkeln i radianer PI/2.</span><span class="sxs-lookup"><span data-stu-id="2bc11-689">hello following example returns hello number of degrees in an angle of PI/2 radians.</span></span>  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 <span data-ttu-id="2bc11-690">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-690">Here is hello result set.</span></span>  
  
```  
[{"$1": 90}]  
```  
  
####  <span data-ttu-id="2bc11-691"><a name="bk_floor"></a>VÅNING</span><span class="sxs-lookup"><span data-stu-id="2bc11-691"><a name="bk_floor"></a> FLOOR</span></span>  
 <span data-ttu-id="2bc11-692">Returnerar hello största heltal som är mindre än eller lika toohello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-692">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-693">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-693">**Syntax**</span></span>  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-694">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-694">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-695">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-695">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-696">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-696">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-697">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-697">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-698">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-698">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-699">hello som följande exempel visar positivt numeriskt negativt, och noll värden med hello VÅNING funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-699">hello following example shows positive numeric, negative, and zero values with hello FLOOR function.</span></span>  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 <span data-ttu-id="2bc11-700">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-700">Here is hello result set.</span></span>  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <span data-ttu-id="2bc11-701"><a name="bk_exp"></a>EXP</span><span class="sxs-lookup"><span data-stu-id="2bc11-701"><a name="bk_exp"></a> EXP</span></span>  
 <span data-ttu-id="2bc11-702">Returnerar hello exponentiell värdet för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-702">Returns hello exponential value of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-703">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-703">**Syntax**</span></span>  
  
```  
EXP (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-704">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-704">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-705">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-705">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-706">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-706">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-707">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-707">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-708">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-708">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-709">hello konstant **e** (2.718281...) är hello basen för den naturliga logaritmen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-709">hello constant **e** (2.718281…), is hello base of natural logarithms.</span></span>  
  
 <span data-ttu-id="2bc11-710">hello e upphöjt till ett tal är hello konstant **e** aktiveras toohello power hello tal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-710">hello exponent of a number is hello constant **e** raised toohello power of hello number.</span></span> <span data-ttu-id="2bc11-711">Till exempel EXP(1.0) = e ^ 1.0 = 2.71828182845905 och EXP(10) = e ^ 10 = 22026.4657948067.</span><span class="sxs-lookup"><span data-stu-id="2bc11-711">For example EXP(1.0) = e^1.0 = 2.71828182845905 and EXP(10) = e^10 = 22026.4657948067.</span></span>  
  
 <span data-ttu-id="2bc11-712">hello e upphöjt till hello naturliga logaritmen för ett tal är hello tal sig själv: EXP (loggning (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="2bc11-712">hello exponential of hello natural logarithm of a number is hello number itself: EXP (LOG (n)) = n.</span></span> <span data-ttu-id="2bc11-713">Och hello naturliga logaritmen för hello e upphöjt till ett tal är hello sig själv: LOGG (EXP (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="2bc11-713">And hello natural logarithm of hello exponential of a number is hello number itself: LOG (EXP (n)) = n.</span></span>  
  
 <span data-ttu-id="2bc11-714">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-714">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-715">hello följande exempel deklarerar en variabel och returnerar hello exponentiell hello specifik variabel (10).</span><span class="sxs-lookup"><span data-stu-id="2bc11-715">hello following example declares a variable and returns hello exponential value of hello specified variable (10).</span></span>  
  
```  
SELECT EXP(10)  
```  
  
 <span data-ttu-id="2bc11-716">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-716">Here is hello result set.</span></span>  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 <span data-ttu-id="2bc11-717">hello returnerar följande exempel hello exponentiell värdet hello naturliga logaritmen för 20 och hello naturliga logaritmen för hello e upphöjt till 20.</span><span class="sxs-lookup"><span data-stu-id="2bc11-717">hello following example returns hello exponential value of hello natural logarithm of 20 and hello natural logarithm of hello exponential of 20.</span></span> <span data-ttu-id="2bc11-718">Eftersom dessa funktioner är inverterade funktioner för varandra, hello returvärdet med avrundning för beräkningar i båda fallen är 20 flyttal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-718">Because these functions are inverse functions of one another, hello return value with rounding for floating point math in both cases is 20.</span></span>  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 <span data-ttu-id="2bc11-719">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-719">Here is hello result set.</span></span>  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <span data-ttu-id="2bc11-720"><a name="bk_log"></a>LOGG</span><span class="sxs-lookup"><span data-stu-id="2bc11-720"><a name="bk_log"></a> LOG</span></span>  
 <span data-ttu-id="2bc11-721">Returnerar hello naturliga logaritmen för hello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-721">Returns hello natural logarithm of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-722">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-722">**Syntax**</span></span>  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 <span data-ttu-id="2bc11-723">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-723">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-724">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-724">Is a numeric expression.</span></span>  
  
-   `base`  
  
     <span data-ttu-id="2bc11-725">Valfritt numeriskt argument som anger hello för hello logaritmen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-725">Optional numeric argument that sets hello base for hello logarithm.</span></span>  
  
 <span data-ttu-id="2bc11-726">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-726">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-727">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-727">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-728">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-728">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-729">Som standard returnerar LOG() hello naturliga logaritmen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-729">By default, LOG() returns hello natural logarithm.</span></span> <span data-ttu-id="2bc11-730">Du kan ändra hello basen för hello logaritmen tooanother värde med hjälp av valfri grundläggande hello-parameter.</span><span class="sxs-lookup"><span data-stu-id="2bc11-730">You can change hello base of hello logarithm tooanother value by using hello optional base parameter.</span></span>  
  
 <span data-ttu-id="2bc11-731">hello naturliga logaritmen är hello logaritmen toohello grundläggande **e**, där **e** är en onormal konstant ungefär samma too2.718281828.</span><span class="sxs-lookup"><span data-stu-id="2bc11-731">hello natural logarithm is hello logarithm toohello base **e**, where **e** is an irrational constant approximately equal too2.718281828.</span></span>  
  
 <span data-ttu-id="2bc11-732">hello naturliga logaritmen för hello e upphöjt till ett tal är hello tal sig själv: LOGG (EXP (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="2bc11-732">hello natural logarithm of hello exponential of a number is hello number itself: LOG( EXP( n ) ) = n.</span></span> <span data-ttu-id="2bc11-733">Och hello e upphöjt till hello naturliga logaritmen för ett tal är hello sig själv: EXP (loggning (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="2bc11-733">And hello exponential of hello natural logarithm of a number is hello number itself: EXP( LOG( n ) ) = n.</span></span>  
  
 <span data-ttu-id="2bc11-734">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-734">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-735">hello följande exempel deklarerar en variabel och returnerar hello logaritmen hello specifik variabel (10).</span><span class="sxs-lookup"><span data-stu-id="2bc11-735">hello following example declares a variable and returns hello logarithm value of hello specified variable (10).</span></span>  
  
```  
SELECT LOG(10)  
```  
  
 <span data-ttu-id="2bc11-736">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-736">Here is hello result set.</span></span>  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 <span data-ttu-id="2bc11-737">hello beräknar följande exempel hello LOGGEN för hello e upphöjt till ett tal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-737">hello following example calculates hello LOG for hello exponent of a number.</span></span>  
  
```  
SELECT EXP(LOG(10))  
```  
  
 <span data-ttu-id="2bc11-738">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-738">Here is hello result set.</span></span>  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <span data-ttu-id="2bc11-739"><a name="bk_log10"></a>LOG10</span><span class="sxs-lookup"><span data-stu-id="2bc11-739"><a name="bk_log10"></a> LOG10</span></span>  
 <span data-ttu-id="2bc11-740">Returnerar hello 10-logaritmen av hello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-740">Returns hello base-10 logarithm of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-741">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-741">**Syntax**</span></span>  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-742">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-742">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-743">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-743">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-744">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-744">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-745">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-745">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-746">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="2bc11-746">**Remarks**</span></span>  
  
 <span data-ttu-id="2bc11-747">hello LOG10 och POWER funktionerna är omvänt relaterade tooone en annan.</span><span class="sxs-lookup"><span data-stu-id="2bc11-747">hello LOG10 and POWER functions are inversely related tooone another.</span></span> <span data-ttu-id="2bc11-748">Till exempel 10 ^ LOG10(n) = n.</span><span class="sxs-lookup"><span data-stu-id="2bc11-748">For example, 10 ^ LOG10(n) = n.</span></span>  
  
 <span data-ttu-id="2bc11-749">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-749">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-750">hello följande exempel deklarerar en variabel och returnerar hello LOG10 hello specifik variabel (100).</span><span class="sxs-lookup"><span data-stu-id="2bc11-750">hello following example declares a variable and returns hello LOG10 value of hello specified variable (100).</span></span>  
  
```  
SELECT LOG10(100)  
```  
  
 <span data-ttu-id="2bc11-751">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-751">Here is hello result set.</span></span>  
  
```  
[{$1: 2}]  
```  
  
####  <span data-ttu-id="2bc11-752"><a name="bk_pi"></a>PI</span><span class="sxs-lookup"><span data-stu-id="2bc11-752"><a name="bk_pi"></a> PI</span></span>  
 <span data-ttu-id="2bc11-753">Returnerar hello konstantvärde pi.</span><span class="sxs-lookup"><span data-stu-id="2bc11-753">Returns hello constant value of PI.</span></span>  
  
 <span data-ttu-id="2bc11-754">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-754">**Syntax**</span></span>  
  
```  
PI ()  
```  
  
 <span data-ttu-id="2bc11-755">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-755">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-756">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-756">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-757">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-757">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-758">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-758">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-759">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-759">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-760">hello returnerar följande exempel värdet PI hello.</span><span class="sxs-lookup"><span data-stu-id="2bc11-760">hello following example returns hello value of PI.</span></span>  
  
```  
SELECT PI()  
```  
  
 <span data-ttu-id="2bc11-761">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-761">Here is hello result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="2bc11-762"><a name="bk_power"></a>POWER</span><span class="sxs-lookup"><span data-stu-id="2bc11-762"><a name="bk_power"></a> POWER</span></span>  
 <span data-ttu-id="2bc11-763">Returnerar hello hello anges värdet uttryck toohello angetts power.</span><span class="sxs-lookup"><span data-stu-id="2bc11-763">Returns hello value of hello specified expression toohello specified power.</span></span>  
  
 <span data-ttu-id="2bc11-764">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-764">**Syntax**</span></span>  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 <span data-ttu-id="2bc11-765">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-765">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-766">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-766">Is a numeric expression.</span></span>  
  
-   `y`  
  
     <span data-ttu-id="2bc11-767">Är hello power toowhich tooraise `numeric_expression`.</span><span class="sxs-lookup"><span data-stu-id="2bc11-767">Is hello power toowhich tooraise `numeric_expression`.</span></span>  
  
 <span data-ttu-id="2bc11-768">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-768">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-769">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-769">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-770">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-770">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-771">hello som följande exempel visar höja toohello tal upphöjt till 3 (nummer hello hello kub).</span><span class="sxs-lookup"><span data-stu-id="2bc11-771">hello following example demonstrates raising a number toohello power of 3 (hello cube of hello number).</span></span>  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 <span data-ttu-id="2bc11-772">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-772">Here is hello result set.</span></span>  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <span data-ttu-id="2bc11-773"><a name="bk_radians"></a>RADIANER</span><span class="sxs-lookup"><span data-stu-id="2bc11-773"><a name="bk_radians"></a> RADIANS</span></span>  
 <span data-ttu-id="2bc11-774">Returnerar radianer när ett numeriskt uttryck i grader anges.</span><span class="sxs-lookup"><span data-stu-id="2bc11-774">Returns radians when a numeric expression, in degrees, is entered.</span></span>  
  
 <span data-ttu-id="2bc11-775">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-775">**Syntax**</span></span>  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-776">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-776">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-777">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-777">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-778">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-778">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-779">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-779">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-780">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-780">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-781">hello följande exempel tar några vinklar som indata och returnerar motsvarande radian värden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-781">hello following example takes a few angles as input and returns their corresponding radian values.</span></span>  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 <span data-ttu-id="2bc11-782">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-782">Here is hello result set.</span></span>  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <span data-ttu-id="2bc11-783"><a name="bk_round"></a>AVRUNDA</span><span class="sxs-lookup"><span data-stu-id="2bc11-783"><a name="bk_round"></a> ROUND</span></span>  
 <span data-ttu-id="2bc11-784">Returnerar ett numeriskt värde avrundade toohello närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-784">Returns a numeric value, rounded toohello closest integer value.</span></span>  
  
 <span data-ttu-id="2bc11-785">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-785">**Syntax**</span></span>  
  
```  
ROUND(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-786">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-786">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-787">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-787">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-788">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-788">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-789">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-789">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-790">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-790">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-791">hello Avrundar följande exempel hello efter positiva och negativa tal toohello närmsta heltal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-791">hello following example rounds hello following positive and negative numbers toohello nearest integer.</span></span>  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 <span data-ttu-id="2bc11-792">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-792">Here is hello result set.</span></span>  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <span data-ttu-id="2bc11-793"><a name="bk_sign"></a>LOGGA</span><span class="sxs-lookup"><span data-stu-id="2bc11-793"><a name="bk_sign"></a> SIGN</span></span>  
 <span data-ttu-id="2bc11-794">Returnerar hello positiv (+ 1), noll (0) eller minustecken (-1) för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-794">Returns hello positive (+1), zero (0), or negative (-1) sign of hello specified numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-795">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-795">**Syntax**</span></span>  
  
```  
SIGN(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-796">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-796">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-797">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-797">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-798">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-798">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-799">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-799">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-800">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-800">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-801">hello returnerar följande exempel hello LOGGA värdena av talen från too2-2.</span><span class="sxs-lookup"><span data-stu-id="2bc11-801">hello following example returns hello SIGN values of numbers from -2 too2.</span></span>  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 <span data-ttu-id="2bc11-802">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-802">Here is hello result set.</span></span>  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <span data-ttu-id="2bc11-803"><a name="bk_sin"></a>SIN</span><span class="sxs-lookup"><span data-stu-id="2bc11-803"><a name="bk_sin"></a> SIN</span></span>  
 <span data-ttu-id="2bc11-804">Returnerar hello trigonometriska värden sinus för hello angiven vinkel i radianer, i hello angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-804">Returns hello trigonometric sine of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="2bc11-805">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-805">**Syntax**</span></span>  
  
```  
SIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-806">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-806">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-807">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-807">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-808">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-808">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-809">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-809">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-810">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-810">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-811">följande exempel hello beräknar hello SIN för hello angivna vinkeln.</span><span class="sxs-lookup"><span data-stu-id="2bc11-811">hello following example calculates hello SIN of hello specified angle.</span></span>  
  
```  
SELECT SIN(45.175643)  
```  
  
 <span data-ttu-id="2bc11-812">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-812">Here is hello result set.</span></span>  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <span data-ttu-id="2bc11-813"><a name="bk_sqrt"></a>ROT</span><span class="sxs-lookup"><span data-stu-id="2bc11-813"><a name="bk_sqrt"></a> SQRT</span></span>  
 <span data-ttu-id="2bc11-814">Returnerar hello kvadratroten ur hello angetts numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-814">Returns hello square root of hello specified numeric value.</span></span>  
  
 <span data-ttu-id="2bc11-815">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-815">**Syntax**</span></span>  
  
```  
SQRT(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-816">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-816">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-817">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-817">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-818">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-818">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-819">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-819">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-820">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-820">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-821">hello returneras följande exempel hello kvadraten rötter av talen 1-3.</span><span class="sxs-lookup"><span data-stu-id="2bc11-821">hello following example returns hello square roots of numbers 1-3.</span></span>  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 <span data-ttu-id="2bc11-822">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-822">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <span data-ttu-id="2bc11-823"><a name="bk_square"></a>RUTA</span><span class="sxs-lookup"><span data-stu-id="2bc11-823"><a name="bk_square"></a> SQUARE</span></span>  
 <span data-ttu-id="2bc11-824">Returnerar hello rektangulär hello angetts numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-824">Returns hello square of hello specified numeric value.</span></span>  
  
 <span data-ttu-id="2bc11-825">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-825">**Syntax**</span></span>  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-826">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-826">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-827">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-827">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-828">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-828">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-829">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-829">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-830">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-830">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-831">hello returneras följande exempel hello kvadraten av talen 1-3.</span><span class="sxs-lookup"><span data-stu-id="2bc11-831">hello following example returns hello squares of numbers 1-3.</span></span>  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 <span data-ttu-id="2bc11-832">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-832">Here is hello result set.</span></span>  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <span data-ttu-id="2bc11-833"><a name="bk_tan"></a>TAN</span><span class="sxs-lookup"><span data-stu-id="2bc11-833"><a name="bk_tan"></a> TAN</span></span>  
 <span data-ttu-id="2bc11-834">Returnerar hello tangens för hello angiven vinkel i radianer, i hello angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="2bc11-834">Returns hello tangent of hello specified angle, in radians, in hello specified expression.</span></span>  
  
 <span data-ttu-id="2bc11-835">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-835">**Syntax**</span></span>  
  
```  
TAN (<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-836">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-836">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-837">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-837">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-838">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-838">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-839">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-839">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-840">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-840">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-841">hello följande exempel beräknas hello tangens för PI () / 2.</span><span class="sxs-lookup"><span data-stu-id="2bc11-841">hello following example calculates hello tangent of PI()/2.</span></span>  
  
```  
SELECT TAN(PI()/2);  
```  
  
 <span data-ttu-id="2bc11-842">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-842">Here is hello result set.</span></span>  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <span data-ttu-id="2bc11-843"><a name="bk_trunc"></a>AVKORTA</span><span class="sxs-lookup"><span data-stu-id="2bc11-843"><a name="bk_trunc"></a> TRUNC</span></span>  
 <span data-ttu-id="2bc11-844">Returnerar ett numeriskt värde, trunkerat toohello närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-844">Returns a numeric value, truncated toohello closest integer value.</span></span>  
  
 <span data-ttu-id="2bc11-845">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-845">**Syntax**</span></span>  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 <span data-ttu-id="2bc11-846">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-846">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="2bc11-847">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-847">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-848">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-848">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-849">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-849">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-850">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-850">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-851">följande exempel hello trunkerar hello efter positiva och negativa tal toohello närmsta heltal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-851">hello following example truncates hello following positive and negative numbers toohello nearest integer value.</span></span>  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 <span data-ttu-id="2bc11-852">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-852">Here is hello result set.</span></span>  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <span data-ttu-id="2bc11-853"><a name="bk_type_checking_functions"></a>Ange kontrollerar funktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-853"><a name="bk_type_checking_functions"></a> Type checking functions</span></span>  
 <span data-ttu-id="2bc11-854">hello följande funktioner stöder typkontroll mot indatavärden och varje returnera ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-854">hello following functions support type checking against input values, and each return a Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="2bc11-855">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="2bc11-855">IS_ARRAY</span></span>](#bk_is_array)|[<span data-ttu-id="2bc11-856">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="2bc11-856">IS_BOOL</span></span>](#bk_is_bool)|[<span data-ttu-id="2bc11-857">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="2bc11-857">IS_DEFINED</span></span>](#bk_is_defined)|  
|[<span data-ttu-id="2bc11-858">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="2bc11-858">IS_NULL</span></span>](#bk_is_null)|[<span data-ttu-id="2bc11-859">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="2bc11-859">IS_NUMBER</span></span>](#bk_is_number)|[<span data-ttu-id="2bc11-860">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="2bc11-860">IS_OBJECT</span></span>](#bk_is_object)|  
|[<span data-ttu-id="2bc11-861">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="2bc11-861">IS_PRIMITIVE</span></span>](#bk_is_primitive)|[<span data-ttu-id="2bc11-862">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="2bc11-862">IS_STRING</span></span>](#bk_is_string)||  
  
####  <span data-ttu-id="2bc11-863"><a name="bk_is_array"></a>IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="2bc11-863"><a name="bk_is_array"></a> IS_ARRAY</span></span>  
 <span data-ttu-id="2bc11-864">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är en matris.</span><span class="sxs-lookup"><span data-stu-id="2bc11-864">Returns a Boolean value indicating if hello type of hello specified expression is an array.</span></span>  
  
 <span data-ttu-id="2bc11-865">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-865">**Syntax**</span></span>  
  
```  
IS_ARRAY(<expression>)  
```  
  
 <span data-ttu-id="2bc11-866">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-866">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-867">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-867">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-868">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-868">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-869">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-869">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-870">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-870">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-871">hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_ARRAY funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-871">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_ARRAY function.</span></span>  
  
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
  
 <span data-ttu-id="2bc11-872">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-872">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <span data-ttu-id="2bc11-873"><a name="bk_is_bool"></a>IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="2bc11-873"><a name="bk_is_bool"></a> IS_BOOL</span></span>  
 <span data-ttu-id="2bc11-874">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-874">Returns a Boolean value indicating if hello type of hello specified expression is a Boolean.</span></span>  
  
 <span data-ttu-id="2bc11-875">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-875">**Syntax**</span></span>  
  
```  
IS_BOOL(<expression>)  
```  
  
 <span data-ttu-id="2bc11-876">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-876">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-877">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-877">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-878">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-878">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-879">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-879">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-880">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-880">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-881">hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_BOOL funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-881">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_BOOL function.</span></span>  
  
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
  
 <span data-ttu-id="2bc11-882">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-882">Here is hello result set.</span></span>  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="2bc11-883"><a name="bk_is_defined"></a>IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="2bc11-883"><a name="bk_is_defined"></a> IS_DEFINED</span></span>  
 <span data-ttu-id="2bc11-884">Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-884">Returns a Boolean indicating if hello property has been assigned a value.</span></span>  
  
 <span data-ttu-id="2bc11-885">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-885">**Syntax**</span></span>  
  
```  
IS_DEFINED(<expression>)  
```  
  
 <span data-ttu-id="2bc11-886">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-886">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-887">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-887">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-888">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-888">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-889">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-889">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-890">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-890">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-891">följande exempel söker efter hello förekomsten av en egenskap i hello hello angetts JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-891">hello following example checks for hello presence of a property within hello specified JSON document.</span></span> <span data-ttu-id="2bc11-892">hello returnerar först SANT eftersom ”a” finns, men hello andra returnerar värdet false eftersom ”b” saknas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-892">hello first returns true since "a" is present, but hello second returns false since "b" is absent.</span></span>  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 <span data-ttu-id="2bc11-893">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-893">Here is hello result set.</span></span>  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <span data-ttu-id="2bc11-894"><a name="bk_is_null"></a>IS_NULL</span><span class="sxs-lookup"><span data-stu-id="2bc11-894"><a name="bk_is_null"></a> IS_NULL</span></span>  
 <span data-ttu-id="2bc11-895">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är null.</span><span class="sxs-lookup"><span data-stu-id="2bc11-895">Returns a Boolean value indicating if hello type of hello specified expression is null.</span></span>  
  
 <span data-ttu-id="2bc11-896">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-896">**Syntax**</span></span>  
  
```  
IS_NULL(<expression>)  
```  
  
 <span data-ttu-id="2bc11-897">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-897">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-898">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-898">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-899">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-899">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-900">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-900">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-901">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-901">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-902">hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_NULL funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-902">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="2bc11-903">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-903">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="2bc11-904"><a name="bk_is_number"></a>IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="2bc11-904"><a name="bk_is_number"></a> IS_NUMBER</span></span>  
 <span data-ttu-id="2bc11-905">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett tal.</span><span class="sxs-lookup"><span data-stu-id="2bc11-905">Returns a Boolean value indicating if hello type of hello specified expression is a number.</span></span>  
  
 <span data-ttu-id="2bc11-906">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-906">**Syntax**</span></span>  
  
```  
IS_NUMBER(<expression>)  
```  
  
 <span data-ttu-id="2bc11-907">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-907">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-908">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-908">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-909">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-909">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-910">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-910">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-911">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-911">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-912">hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_NULL funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-912">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="2bc11-913">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-913">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="2bc11-914"><a name="bk_is_object"></a>IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="2bc11-914"><a name="bk_is_object"></a> IS_OBJECT</span></span>  
 <span data-ttu-id="2bc11-915">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-915">Returns a Boolean value indicating if hello type of hello specified expression is a JSON object.</span></span>  
  
 <span data-ttu-id="2bc11-916">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-916">**Syntax**</span></span>  
  
```  
IS_OBJECT(<expression>)  
```  
  
 <span data-ttu-id="2bc11-917">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-917">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-918">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-918">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-919">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-919">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-920">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-920">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-921">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-921">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-922">hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_OBJECT funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-922">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_OBJECT function.</span></span>  
  
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
  
 <span data-ttu-id="2bc11-923">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-923">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <span data-ttu-id="2bc11-924"><a name="bk_is_primitive"></a>IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="2bc11-924"><a name="bk_is_primitive"></a> IS_PRIMITIVE</span></span>  
 <span data-ttu-id="2bc11-925">Returnerar ett booleskt värde som anger om hello hello angetts uttrycket är en primitiv (string, Boolean, numeriska eller null).</span><span class="sxs-lookup"><span data-stu-id="2bc11-925">Returns a Boolean value indicating if hello type of hello specified expression is a primitive (string, Boolean, numeric or null).</span></span>  
  
 <span data-ttu-id="2bc11-926">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-926">**Syntax**</span></span>  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 <span data-ttu-id="2bc11-927">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-927">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-928">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-928">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-929">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-929">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-930">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-930">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-931">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-931">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-932">hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_PRIMITIVE funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-932">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_PRIMITIVE function.</span></span>  
  
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
  
 <span data-ttu-id="2bc11-933">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-933">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <span data-ttu-id="2bc11-934"><a name="bk_is_string"></a>IS_STRING</span><span class="sxs-lookup"><span data-stu-id="2bc11-934"><a name="bk_is_string"></a> IS_STRING</span></span>  
 <span data-ttu-id="2bc11-935">Returnerar ett booleskt värde som anger om hello hello angetts uttryck är en sträng.</span><span class="sxs-lookup"><span data-stu-id="2bc11-935">Returns a Boolean value indicating if hello type of hello specified expression is a string.</span></span>  
  
 <span data-ttu-id="2bc11-936">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-936">**Syntax**</span></span>  
  
```  
IS_STRING(<expression>)  
```  
  
 <span data-ttu-id="2bc11-937">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-937">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="2bc11-938">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-938">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-939">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-939">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-940">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-940">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-941">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-941">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-942">hello kontrollerar följande exempel objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hello IS_STRING funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-942">hello following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using hello IS_STRING function.</span></span>  
  
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
  
 <span data-ttu-id="2bc11-943">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-943">Here is hello result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <span data-ttu-id="2bc11-944"><a name="bk_string_functions"></a>Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-944"><a name="bk_string_functions"></a> String functions</span></span>  
 <span data-ttu-id="2bc11-945">hello följande skalärfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-945">hello following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="2bc11-946">CONCAT</span><span class="sxs-lookup"><span data-stu-id="2bc11-946">CONCAT</span></span>](#bk_concat)|[<span data-ttu-id="2bc11-947">INNEHÅLLER</span><span class="sxs-lookup"><span data-stu-id="2bc11-947">CONTAINS</span></span>](#bk_contains)|[<span data-ttu-id="2bc11-948">ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="2bc11-948">ENDSWITH</span></span>](#bk_endswith)|  
|[<span data-ttu-id="2bc11-949">INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="2bc11-949">INDEX_OF</span></span>](#bk_index_of)|[<span data-ttu-id="2bc11-950">VÄNSTER</span><span class="sxs-lookup"><span data-stu-id="2bc11-950">LEFT</span></span>](#bk_left)|[<span data-ttu-id="2bc11-951">LÄNGD</span><span class="sxs-lookup"><span data-stu-id="2bc11-951">LENGTH</span></span>](#bk_length)|  
|[<span data-ttu-id="2bc11-952">LÄGRE</span><span class="sxs-lookup"><span data-stu-id="2bc11-952">LOWER</span></span>](#bk_lower)|[<span data-ttu-id="2bc11-953">LTRIM</span><span class="sxs-lookup"><span data-stu-id="2bc11-953">LTRIM</span></span>](#bk_ltrim)|[<span data-ttu-id="2bc11-954">ERSÄTT</span><span class="sxs-lookup"><span data-stu-id="2bc11-954">REPLACE</span></span>](#bk_replace)|  
|[<span data-ttu-id="2bc11-955">Replikera</span><span class="sxs-lookup"><span data-stu-id="2bc11-955">REPLICATE</span></span>](#bk_replicate)|[<span data-ttu-id="2bc11-956">OMVÄND</span><span class="sxs-lookup"><span data-stu-id="2bc11-956">REVERSE</span></span>](#bk_reverse)|[<span data-ttu-id="2bc11-957">HÖGER</span><span class="sxs-lookup"><span data-stu-id="2bc11-957">RIGHT</span></span>](#bk_right)|  
|[<span data-ttu-id="2bc11-958">RTRIM</span><span class="sxs-lookup"><span data-stu-id="2bc11-958">RTRIM</span></span>](#bk_rtrim)|[<span data-ttu-id="2bc11-959">STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="2bc11-959">STARTSWITH</span></span>](#bk_startswith)|[<span data-ttu-id="2bc11-960">DELSTRÄNGEN</span><span class="sxs-lookup"><span data-stu-id="2bc11-960">SUBSTRING</span></span>](#bk_substring)|  
|[<span data-ttu-id="2bc11-961">ÖVRE</span><span class="sxs-lookup"><span data-stu-id="2bc11-961">UPPER</span></span>](#bk_upper)|||  
  
####  <span data-ttu-id="2bc11-962"><a name="bk_concat"></a>CONCAT</span><span class="sxs-lookup"><span data-stu-id="2bc11-962"><a name="bk_concat"></a> CONCAT</span></span>  
 <span data-ttu-id="2bc11-963">Returnerar en sträng som är hello resultat sammanfoga två eller flera strängvärden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-963">Returns a string that is hello result of concatenating two or more string values.</span></span>  
  
 <span data-ttu-id="2bc11-964">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-964">**Syntax**</span></span>  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 <span data-ttu-id="2bc11-965">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-965">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-966">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-966">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-967">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-967">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-968">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-968">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-969">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-969">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-970">följande exempel returnerar hello sammanfogas sträng med hello hello värden har angetts.</span><span class="sxs-lookup"><span data-stu-id="2bc11-970">hello following example returns hello concatenated string of hello specified values.</span></span>  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 <span data-ttu-id="2bc11-971">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-971">Here is hello result set.</span></span>  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <span data-ttu-id="2bc11-972"><a name="bk_contains"></a>INNEHÅLLER</span><span class="sxs-lookup"><span data-stu-id="2bc11-972"><a name="bk_contains"></a> CONTAINS</span></span>  
 <span data-ttu-id="2bc11-973">Returnerar ett booleskt värde som anger om hello första stränguttryck innehåller hello andra.</span><span class="sxs-lookup"><span data-stu-id="2bc11-973">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span>  
  
 <span data-ttu-id="2bc11-974">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-974">**Syntax**</span></span>  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="2bc11-975">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-975">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-976">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-976">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-977">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-977">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-978">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-978">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-979">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-979">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-980">hello kontrollerar följande exempel om ”abc” innehåller ”ab” och ”d”.</span><span class="sxs-lookup"><span data-stu-id="2bc11-980">hello following example checks if "abc" contains "ab" and contains "d".</span></span>  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 <span data-ttu-id="2bc11-981">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-981">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="2bc11-982"><a name="bk_endswith"></a>ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="2bc11-982"><a name="bk_endswith"></a> ENDSWITH</span></span>  
 <span data-ttu-id="2bc11-983">Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra.</span><span class="sxs-lookup"><span data-stu-id="2bc11-983">Returns a Boolean indicating whether hello first string expression ends with hello second.</span></span>  
  
 <span data-ttu-id="2bc11-984">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-984">**Syntax**</span></span>  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="2bc11-985">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-985">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-986">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-986">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-987">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-987">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-988">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-988">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-989">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-989">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-990">hello returneras följande exempel hello ”abc” slutar med ”b” och ”bc”.</span><span class="sxs-lookup"><span data-stu-id="2bc11-990">hello following example returns hello "abc" ends with "b" and "bc".</span></span>  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 <span data-ttu-id="2bc11-991">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-991">Here is hello result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="2bc11-992"><a name="bk_index_of"></a>INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="2bc11-992"><a name="bk_index_of"></a> INDEX_OF</span></span>  
 <span data-ttu-id="2bc11-993">Returnerar hello startposition för hello första förekomsten av hello andra stränguttryck inom hello första angivet stränguttryck eller -1 om hello strängen inte hittas.</span><span class="sxs-lookup"><span data-stu-id="2bc11-993">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span>  
  
 <span data-ttu-id="2bc11-994">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-994">**Syntax**</span></span>  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="2bc11-995">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-995">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-996">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-996">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-997">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-997">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-998">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-998">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-999">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-999">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1000">hello returnerar följande exempel hello index med olika delsträngar inuti ”abc”.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1000">hello following example returns hello index of various substrings inside "abc".</span></span>  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 <span data-ttu-id="2bc11-1001">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1001">Here is hello result set.</span></span>  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <span data-ttu-id="2bc11-1002"><a name="bk_left"></a>VÄNSTER</span><span class="sxs-lookup"><span data-stu-id="2bc11-1002"><a name="bk_left"></a> LEFT</span></span>  
 <span data-ttu-id="2bc11-1003">Returnerar hello vänstra del av en sträng med hello angivna antalet tecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1003">Returns hello left part of a string with hello specified number of characters.</span></span>  
  
 <span data-ttu-id="2bc11-1004">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1004">**Syntax**</span></span>  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="2bc11-1005">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1005">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1006">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1006">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="2bc11-1007">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1007">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-1008">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1008">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1009">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1009">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1010">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1010">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1011">hello följande exempel returneras hello kvar en del av ”abc” för olika värden för längd.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1011">hello following example returns hello left part of "abc" for various length values.</span></span>  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 <span data-ttu-id="2bc11-1012">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1012">Here is hello result set.</span></span>  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <span data-ttu-id="2bc11-1013"><a name="bk_length"></a>LÄNGD</span><span class="sxs-lookup"><span data-stu-id="2bc11-1013"><a name="bk_length"></a> LENGTH</span></span>  
 <span data-ttu-id="2bc11-1014">Returnerar hello antalet tecken i hello angetts stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1014">Returns hello number of characters of hello specified string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1015">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1015">**Syntax**</span></span>  
  
```  
LENGTH(<str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1016">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1016">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1017">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1017">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1018">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1018">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1019">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1019">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1020">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1020">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1021">hello returneras följande exempel hello stränglängden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1021">hello following example returns hello length of a string.</span></span>  
  
```  
SELECT LENGTH("abc")  
```  
  
 <span data-ttu-id="2bc11-1022">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1022">Here is hello result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="2bc11-1023"><a name="bk_lower"></a>LÄGRE</span><span class="sxs-lookup"><span data-stu-id="2bc11-1023"><a name="bk_lower"></a> LOWER</span></span>  
 <span data-ttu-id="2bc11-1024">Returnerar ett stränguttryck efter konvertering versal data toolowercase.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1024">Returns a string expression after converting uppercase character data toolowercase.</span></span>  
  
 <span data-ttu-id="2bc11-1025">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1025">**Syntax**</span></span>  
  
```  
LOWER(<str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1026">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1026">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1027">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1027">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1028">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1028">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1029">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1029">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1030">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1030">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1031">följande exempel visar hur hello toouse längst ned i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1031">hello following example shows how toouse LOWER in a query.</span></span>  
  
```  
SELECT LOWER("Abc")  
```  
  
 <span data-ttu-id="2bc11-1032">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1032">Here is hello result set.</span></span>  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <span data-ttu-id="2bc11-1033"><a name="bk_ltrim"></a>LTRIM</span><span class="sxs-lookup"><span data-stu-id="2bc11-1033"><a name="bk_ltrim"></a> LTRIM</span></span>  
 <span data-ttu-id="2bc11-1034">Returnerar ett stränguttryck när den tar bort inledande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1034">Returns a string expression after it removes leading blanks.</span></span>  
  
 <span data-ttu-id="2bc11-1035">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1035">**Syntax**</span></span>  
  
```  
LTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1036">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1036">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1037">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1037">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1038">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1038">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1039">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1039">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1040">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1040">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1041">följande exempel visar hur hello toouse LTRIM i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1041">hello following example shows how toouse LTRIM inside a query.</span></span>  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 <span data-ttu-id="2bc11-1042">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1042">Here is hello result set.</span></span>  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <span data-ttu-id="2bc11-1043"><a name="bk_replace"></a>ERSÄTT</span><span class="sxs-lookup"><span data-stu-id="2bc11-1043"><a name="bk_replace"></a> REPLACE</span></span>  
 <span data-ttu-id="2bc11-1044">Ersätter alla förekomster av ett angivet strängvärde med ett annat värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1044">Replaces all occurrences of a specified string value with another string value.</span></span>  
  
 <span data-ttu-id="2bc11-1045">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1045">**Syntax**</span></span>  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1046">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1046">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1047">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1047">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1048">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1048">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1049">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1049">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1050">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1050">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1051">hello som följande exempel visar hur toouse Ersätt i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1051">hello following example shows how toouse REPLACE in a query.</span></span>  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 <span data-ttu-id="2bc11-1052">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1052">Here is hello result set.</span></span>  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <span data-ttu-id="2bc11-1053"><a name="bk_replicate"></a>REPLIKERA</span><span class="sxs-lookup"><span data-stu-id="2bc11-1053"><a name="bk_replicate"></a> REPLICATE</span></span>  
 <span data-ttu-id="2bc11-1054">Upprepar ett strängvärde till ett angivet antal gånger.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1054">Repeats a string value a specified number of times.</span></span>  
  
 <span data-ttu-id="2bc11-1055">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1055">**Syntax**</span></span>  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="2bc11-1056">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1056">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1057">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1057">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="2bc11-1058">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1058">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-1059">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1059">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1060">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1060">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1061">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1061">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1062">hello som följande exempel visar hur toouse replikeras i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1062">hello following example shows how toouse REPLICATE in a query.</span></span>  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 <span data-ttu-id="2bc11-1063">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1063">Here is hello result set.</span></span>  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <span data-ttu-id="2bc11-1064"><a name="bk_reverse"></a>OMVÄND</span><span class="sxs-lookup"><span data-stu-id="2bc11-1064"><a name="bk_reverse"></a> REVERSE</span></span>  
 <span data-ttu-id="2bc11-1065">Returnerar hello omvänd ordning i ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1065">Returns hello reverse order of a string value.</span></span>  
  
 <span data-ttu-id="2bc11-1066">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1066">**Syntax**</span></span>  
  
```  
REVERSE(<str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1067">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1067">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1068">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1068">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1069">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1069">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1070">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1070">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1071">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1071">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1072">hello som följande exempel visar hur toouse OMVÄND i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1072">hello following example shows how toouse REVERSE in a query.</span></span>  
  
```  
SELECT REVERSE("Abc")  
```  
  
 <span data-ttu-id="2bc11-1073">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1073">Here is hello result set.</span></span>  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <span data-ttu-id="2bc11-1074"><a name="bk_right"></a>HÖGER</span><span class="sxs-lookup"><span data-stu-id="2bc11-1074"><a name="bk_right"></a> RIGHT</span></span>  
 <span data-ttu-id="2bc11-1075">Returnerar hello just en del av en sträng med hello angivet antal tecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1075">Returns hello right part of a string with hello specified number of characters.</span></span>  
  
 <span data-ttu-id="2bc11-1076">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1076">**Syntax**</span></span>  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="2bc11-1077">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1077">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1078">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1078">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="2bc11-1079">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1079">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-1080">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1080">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1081">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1081">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1082">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1082">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1083">hello returneras följande exempel hello högra delen av ”abc” för olika värden för längd.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1083">hello following example returns hello right part of "abc" for various length values.</span></span>  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 <span data-ttu-id="2bc11-1084">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1084">Here is hello result set.</span></span>  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <span data-ttu-id="2bc11-1085"><a name="bk_rtrim"></a>RTRIM</span><span class="sxs-lookup"><span data-stu-id="2bc11-1085"><a name="bk_rtrim"></a> RTRIM</span></span>  
 <span data-ttu-id="2bc11-1086">Returnerar ett stränguttryck när den tar bort avslutande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1086">Returns a string expression after it removes trailing blanks.</span></span>  
  
 <span data-ttu-id="2bc11-1087">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1087">**Syntax**</span></span>  
  
```  
RTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1088">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1088">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1089">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1089">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1090">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1090">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1091">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1091">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1092">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1092">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1093">följande exempel visar hur hello toouse RTRIM i en fråga.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1093">hello following example shows how toouse RTRIM inside a query.</span></span>  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 <span data-ttu-id="2bc11-1094">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1094">Here is hello result set.</span></span>  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <span data-ttu-id="2bc11-1095"><a name="bk_startswith"></a>STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="2bc11-1095"><a name="bk_startswith"></a> STARTSWITH</span></span>  
 <span data-ttu-id="2bc11-1096">Returnerar ett booleskt värde som anger om hello första stränguttryck börjar med hello andra.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1096">Returns a Boolean indicating whether hello first string expression starts with hello second.</span></span>  
  
 <span data-ttu-id="2bc11-1097">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1097">**Syntax**</span></span>  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1098">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1098">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1099">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1099">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1100">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1100">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1101">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1101">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-1102">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1102">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1103">hello följande exempel kontrollerar om hello strängen ”abc” börjar med ”b” och ”a”.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1103">hello following example checks if hello string "abc" begins with "b" and "a".</span></span>  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 <span data-ttu-id="2bc11-1104">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1104">Here is hello result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="2bc11-1105"><a name="bk_substring"></a>DELSTRÄNGEN</span><span class="sxs-lookup"><span data-stu-id="2bc11-1105"><a name="bk_substring"></a> SUBSTRING</span></span>  
 <span data-ttu-id="2bc11-1106">Returnerar en del av ett stränguttryck som börjar vid hello angetts nollbaserade teckenposition och fortsätter toohello angiven längd eller toohello slutet av hello strängen.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1106">Returns part of a string expression starting at hello specified character zero-based position and continues toohello specified length, or toohello end of hello string.</span></span>  
  
 <span data-ttu-id="2bc11-1107">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1107">**Syntax**</span></span>  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="2bc11-1108">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1108">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1109">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1109">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="2bc11-1110">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1110">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-1111">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1111">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1112">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1112">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1113">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1113">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1114">hello följande exempel returneras hello delsträngen ”ABC” vid 1 och en längd på 1 tecken.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1114">hello following example returns hello substring of "abc" starting at 1 and for a length of 1 character.</span></span>  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 <span data-ttu-id="2bc11-1115">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1115">Here is hello result set.</span></span>  
  
```  
[{"$1": "b"}]  
```  
  
####  <span data-ttu-id="2bc11-1116"><a name="bk_upper"></a>ÖVRE</span><span class="sxs-lookup"><span data-stu-id="2bc11-1116"><a name="bk_upper"></a> UPPER</span></span>  
 <span data-ttu-id="2bc11-1117">Returnerar ett stränguttryck efter konvertering gemen data toouppercase.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1117">Returns a string expression after converting lowercase character data toouppercase.</span></span>  
  
 <span data-ttu-id="2bc11-1118">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1118">**Syntax**</span></span>  
  
```  
UPPER(<str_expr>)  
```  
  
 <span data-ttu-id="2bc11-1119">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1119">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="2bc11-1120">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1120">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1121">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1121">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1122">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1122">Returns a string expression.</span></span>  
  
 <span data-ttu-id="2bc11-1123">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1123">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1124">följande exempel visar hur hello toouse längst upp i en fråga</span><span class="sxs-lookup"><span data-stu-id="2bc11-1124">hello following example shows how toouse UPPER in a query</span></span>  
  
```  
SELECT UPPER("Abc")  
```  
  
 <span data-ttu-id="2bc11-1125">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1125">Here is hello result set.</span></span>  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <span data-ttu-id="2bc11-1126"><a name="bk_array_functions"></a>Matrisfunktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-1126"><a name="bk_array_functions"></a> Array functions</span></span>  
 <span data-ttu-id="2bc11-1127">hello följande skalärfunktioner utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde</span><span class="sxs-lookup"><span data-stu-id="2bc11-1127">hello following scalar functions perform an operation on an array input value and return numeric, Boolean or array value</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="2bc11-1128">ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="2bc11-1128">ARRAY_CONCAT</span></span>](#bk_array_concat)|[<span data-ttu-id="2bc11-1129">ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="2bc11-1129">ARRAY_CONTAINS</span></span>](#bk_array_contains)|[<span data-ttu-id="2bc11-1130">ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="2bc11-1130">ARRAY_LENGTH</span></span>](#bk_array_length)|  
|[<span data-ttu-id="2bc11-1131">ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="2bc11-1131">ARRAY_SLICE</span></span>](#bk_array_slice)|||  
  
####  <span data-ttu-id="2bc11-1132"><a name="bk_array_concat"></a>ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="2bc11-1132"><a name="bk_array_concat"></a> ARRAY_CONCAT</span></span>  
 <span data-ttu-id="2bc11-1133">Returnerar en matris som är hello resultat sammanfoga två eller flera matrisvärden.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1133">Returns an array that is hello result of concatenating two or more array values.</span></span>  
  
 <span data-ttu-id="2bc11-1134">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1134">**Syntax**</span></span>  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 <span data-ttu-id="2bc11-1135">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1135">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="2bc11-1136">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1136">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="2bc11-1137">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1137">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1138">Returnerar en matrisuttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1138">Returns an array expression.</span></span>  
  
 <span data-ttu-id="2bc11-1139">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1139">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1140">Hej följande exempel hur tooconcatenate två matriser.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1140">hello following example how tooconcatenate two arrays.</span></span>  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 <span data-ttu-id="2bc11-1141">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1141">Here is hello result set.</span></span>  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <span data-ttu-id="2bc11-1142"><a name="bk_array_contains"></a>ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="2bc11-1142"><a name="bk_array_contains"></a> ARRAY_CONTAINS</span></span>  
 <span data-ttu-id="2bc11-1143">Returnerar ett booleskt värde som anger om hello matris innehåller hello anges värdet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1143">Returns a Boolean indicating whether hello array contains hello specified value.</span></span>  
  
 <span data-ttu-id="2bc11-1144">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1144">**Syntax**</span></span>  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 <span data-ttu-id="2bc11-1145">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1145">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="2bc11-1146">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1146">Is any valid array expression.</span></span>  
  
-   `expr`  
  
     <span data-ttu-id="2bc11-1147">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1147">Is any valid expression.</span></span>  
  
 <span data-ttu-id="2bc11-1148">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1148">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1149">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1149">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="2bc11-1150">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1150">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1151">följande exempel på hur hello toocheck för medlemskap i en matris med ARRAY_CONTAINS.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1151">hello following example how toocheck for membership in an array using ARRAY_CONTAINS.</span></span>  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 <span data-ttu-id="2bc11-1152">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1152">Here is hello result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="2bc11-1153"><a name="bk_array_length"></a>ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="2bc11-1153"><a name="bk_array_length"></a> ARRAY_LENGTH</span></span>  
 <span data-ttu-id="2bc11-1154">Returnerar hello antalet element i hello angetts matrisuttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1154">Returns hello number of elements of hello specified array expression.</span></span>  
  
 <span data-ttu-id="2bc11-1155">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1155">**Syntax**</span></span>  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 <span data-ttu-id="2bc11-1156">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1156">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="2bc11-1157">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1157">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="2bc11-1158">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1158">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1159">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1159">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-1160">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1160">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1161">Hej följande exempel på hur tooget hello längden på en matris med ARRAY_LENGTH.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1161">hello following example how tooget hello length of an array using ARRAY_LENGTH.</span></span>  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 <span data-ttu-id="2bc11-1162">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1162">Here is hello result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="2bc11-1163"><a name="bk_array_slice"></a>ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="2bc11-1163"><a name="bk_array_slice"></a> ARRAY_SLICE</span></span>  
 <span data-ttu-id="2bc11-1164">Returnerar en del av ett matrisuttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1164">Returns part of an array expression.</span></span>
  
 <span data-ttu-id="2bc11-1165">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1165">**Syntax**</span></span>  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="2bc11-1166">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1166">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="2bc11-1167">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1167">Is any valid array expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="2bc11-1168">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1168">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="2bc11-1169">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1169">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1170">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1170">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="2bc11-1171">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1171">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1172">följande exempel på hur hello tooget en del av en matris med ARRAY_SLICE.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1172">hello following example how tooget a part of an array using ARRAY_SLICE.</span></span>  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 <span data-ttu-id="2bc11-1173">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1173">Here is hello result set.</span></span>  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <span data-ttu-id="2bc11-1174"><a name="bk_spatial_functions"></a>Spatial funktioner</span><span class="sxs-lookup"><span data-stu-id="2bc11-1174"><a name="bk_spatial_functions"></a> Spatial functions</span></span>  
 <span data-ttu-id="2bc11-1175">hello följande skalärfunktioner utföra en åtgärd på ett indatavärde Rumsobjektet och returnera ett numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1175">hello following scalar functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="2bc11-1176">ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="2bc11-1176">ST_DISTANCE</span></span>](#bk_st_distance)|[<span data-ttu-id="2bc11-1177">ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="2bc11-1177">ST_WITHIN</span></span>](#bk_st_within)|[<span data-ttu-id="2bc11-1178">ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="2bc11-1178">ST_INTERSECTS</span></span>](#bk_st_intersects)|[<span data-ttu-id="2bc11-1179">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="2bc11-1179">ST_ISVALID</span></span>](#bk_st_isvalid)|  
|[<span data-ttu-id="2bc11-1180">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="2bc11-1180">ST_ISVALIDDETAILED</span></span>](#bk_st_isvaliddetailed)|||  
  
####  <span data-ttu-id="2bc11-1181"><a name="bk_st_distance"></a>ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="2bc11-1181"><a name="bk_st_distance"></a> ST_DISTANCE</span></span>  
 <span data-ttu-id="2bc11-1182">Returnerar hello avståndet mellan hello två GeoJSON punkt, Polygon eller LineString uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1182">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span>  
  
 <span data-ttu-id="2bc11-1183">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1183">**Syntax**</span></span>  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="2bc11-1184">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1184">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="2bc11-1185">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1185">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="2bc11-1186">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1186">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1187">Returnerar ett stränguttryck som innehåller hello avstånd.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1187">Returns a numeric expression containing hello distance.</span></span> <span data-ttu-id="2bc11-1188">Detta uttrycks i mätare för hello standard referenssystem.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1188">This is expressed in meters for hello default reference system.</span></span>  
  
 <span data-ttu-id="2bc11-1189">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1189">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1190">följande exempel visar hur hello tooreturn alla family dokument som ligger inom 30 km av hello anges platsen med hjälp av hello ST_DISTANCE inbyggd funktion.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1190">hello following example shows how tooreturn all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> <span data-ttu-id="2bc11-1191">.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1191">.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 <span data-ttu-id="2bc11-1192">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1192">Here is hello result set.</span></span>  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <span data-ttu-id="2bc11-1193"><a name="bk_st_within"></a>ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="2bc11-1193"><a name="bk_st_within"></a> ST_WITHIN</span></span>  
 <span data-ttu-id="2bc11-1194">Returnerar ett booleskt uttryck som anger om hello GeoJSON (Point, LineString eller Polygon) det angivna objektet i hello första argumentet ligger inom hello GeoJSON (Point, LineString eller Polygon) i hello andra argumentet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1194">Returns a Boolean expression indicating whether hello GeoJSON object (Point, Polygon, or LineString) specified in hello first argument is within hello GeoJSON (Point, Polygon, or LineString) in hello second argument.</span></span>  
  
 <span data-ttu-id="2bc11-1195">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1195">**Syntax**</span></span>  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="2bc11-1196">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1196">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="2bc11-1197">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1197">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="2bc11-1198">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1198">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="2bc11-1199">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1199">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1200">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1200">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="2bc11-1201">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1201">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1202">hello som följande exempel visar hur toofind alla familj dokument inom en polygon med ST_WITHIN.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1202">hello following example shows how toofind all family documents within a polygon using ST_WITHIN.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="2bc11-1203">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1203">Here is hello result set.</span></span>  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <span data-ttu-id="2bc11-1204"><a name="bk_st_intersects"></a>ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="2bc11-1204"><a name="bk_st_intersects"></a> ST_INTERSECTS</span></span>  
 <span data-ttu-id="2bc11-1205">Returnerar ett booleskt uttryck som anger om hello GeoJSON (Point, LineString eller Polygon) det angivna objektet i hello första argumentet korsar hello GeoJSON (Point, LineString eller Polygon) i hello andra argumentet.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1205">Returns a Boolean expression indicating whether hello GeoJSON object (Point, Polygon, or LineString) specified in hello first argument intersects hello GeoJSON (Point, Polygon, or LineString) in hello second argument.</span></span>  
  
 <span data-ttu-id="2bc11-1206">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1206">**Syntax**</span></span>  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="2bc11-1207">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1207">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="2bc11-1208">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1208">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="2bc11-1209">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1209">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="2bc11-1210">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1210">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1211">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1211">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="2bc11-1212">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1212">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1213">följande exempel visar hur hello toofind alla områden som hello angivna polygon.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1213">hello following example shows how toofind all areas that intersects with hello given polygon.</span></span>  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="2bc11-1214">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1214">Here is hello result set.</span></span>  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <span data-ttu-id="2bc11-1215"><a name="bk_st_isvalid"></a>ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="2bc11-1215"><a name="bk_st_isvalid"></a> ST_ISVALID</span></span>  
 <span data-ttu-id="2bc11-1216">Returnerar ett booleskt värde som anger om hello angiven GeoJSON punkt, Polygon eller LineString uttryck är giltig.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1216">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span>  
  
 <span data-ttu-id="2bc11-1217">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1217">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="2bc11-1218">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1218">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="2bc11-1219">Är ett giltigt GeoJSON punkt, Polygon eller LineString uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1219">Is any valid GeoJSON Point, Polygon, or LineString expression.</span></span>  
  
 <span data-ttu-id="2bc11-1220">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1220">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1221">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1221">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="2bc11-1222">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1222">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1223">följande exempel visar hur hello toocheck om en är giltig med ST_VALID.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1223">hello following example shows how toocheck if a point is valid using ST_VALID.</span></span>  
  
 <span data-ttu-id="2bc11-1224">Den här platsen har till exempel en latitud-värde som inte är i hello giltiga värdeintervallet [-90, 90], så hello frågan returnerar FALSKT.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1224">For example, this point has a latitude value that's not in hello valid range of values [-90, 90], so hello query returns false.</span></span>  
  
 <span data-ttu-id="2bc11-1225">För polygoner hello GeoJSON specifikationen kräver hello senaste koordinaten par angivna att hello samma som hello först toocreate en sluten form.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1225">For polygons, hello GeoJSON specification requires that hello last coordinate pair provided should be hello same as hello first, toocreate a closed shape.</span></span> <span data-ttu-id="2bc11-1226">Punkter i en polygon måste anges i följd medurs ordning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1226">Points within a polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="2bc11-1227">En polygon som angetts i medurs ordning representerar hello inversen av hello region i den.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1227">A polygon specified in clockwise order represents hello inverse of hello region within it.</span></span>  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 <span data-ttu-id="2bc11-1228">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1228">Here is hello result set.</span></span>  
  
```  
[{ "$1": false }]  
```  
  
####  <span data-ttu-id="2bc11-1229"><a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="2bc11-1229"><a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED</span></span>  
 <span data-ttu-id="2bc11-1230">Returnerar ett JSON-värde som innehåller ett booleskt värde om hello angivna GeoJSON punkt, Polygon eller LineString uttrycket är giltig och om det är ogiltig, dessutom hello orsak som ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1230">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span>  
  
 <span data-ttu-id="2bc11-1231">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1231">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="2bc11-1232">**Argument**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1232">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="2bc11-1233">Är ett giltigt GeoJSON punkt eller polygon uttryck.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1233">Is any valid GeoJSON point or polygon expression.</span></span>  
  
 <span data-ttu-id="2bc11-1234">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1234">**Return Types**</span></span>  
  
 <span data-ttu-id="2bc11-1235">Returnerar ett JSON-värde som innehåller ett booleskt värde om hello anges GeoJSON punkt eller polygon uttryck är giltigt och om det är ogiltig, dessutom hello orsak som ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1235">Returns a JSON value containing a Boolean value if hello specified GeoJSON point or polygon expression is valid, and if invalid, additionally hello reason as a string value.</span></span>  
  
 <span data-ttu-id="2bc11-1236">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="2bc11-1236">**Examples**</span></span>  
  
 <span data-ttu-id="2bc11-1237">följande exempel på hur hello toocheck giltig (med information) med hjälp av ST_ISVALIDDETAILED.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1237">hello following example how toocheck validity (with details) using ST_ISVALIDDETAILED.</span></span>  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 <span data-ttu-id="2bc11-1238">Här är hello resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2bc11-1238">Here is hello result set.</span></span>  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a polygon must have hello same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a><span data-ttu-id="2bc11-1239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2bc11-1239">Next steps</span></span>  
 <span data-ttu-id="2bc11-1240">[SQL-syntax och SQL-fråga för Azure Cosmos DB](documentdb-sql-query.md) </span><span class="sxs-lookup"><span data-stu-id="2bc11-1240">[SQL syntax and SQL query for Azure Cosmos DB](documentdb-sql-query.md) </span></span>  
 [<span data-ttu-id="2bc11-1241">Azure DB Cosmos-dokumentation</span><span class="sxs-lookup"><span data-stu-id="2bc11-1241">Azure Cosmos DB documentation</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
