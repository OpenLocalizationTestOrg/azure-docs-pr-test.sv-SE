---
title: 'Azure Cosmos DB DocumentDB API: SQL-syntaxen | Microsoft Docs'
description: "I referensdokumentationen för Azure Cosmos DB DocumentDB API SQL-frågespråket."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 06/13/2017
ms.author: mimig
ms.openlocfilehash: 63b2d20c74df4fd6173994ee1a727594ba8afba3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-documentdb-api-sql-syntax-reference"></a><span data-ttu-id="f7992-103">Azure Cosmos DB DocumentDB API: Referens SQL-syntax</span><span class="sxs-lookup"><span data-stu-id="f7992-103">Azure Cosmos DB DocumentDB API: SQL syntax reference</span></span>

<span data-ttu-id="f7992-104">API: et för Azure Cosmos DB DocumentDB stöder förfrågningar till dokument med en bekant SQL (Structured Query Language) som grammatik över hierarkiska JSON-dokument utan explicita schema eller att sekundärindex.</span><span class="sxs-lookup"><span data-stu-id="f7992-104">The Azure Cosmos DB DocumentDB API supports querying documents using a familiar SQL (Structured Query Language) like grammar over hierarchical JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> <span data-ttu-id="f7992-105">Det här avsnittet finns i referensdokumentationen för frågespråket i DocumentDB API SQL.</span><span class="sxs-lookup"><span data-stu-id="f7992-105">This topic provides reference documentation for the DocumentDB API SQL query language.</span></span>

<span data-ttu-id="f7992-106">En genomgång av DocumentDB API SQL-frågespråket finns [SQL-frågor för Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="f7992-106">For a walkthrough of the DocumentDB API SQL query language, see [SQL queries for Azure Cosmos DB DocumentDB API](documentdb-sql-query.md).</span></span>  
  
<span data-ttu-id="f7992-107">Vi ber dig att besöka även den [Query Playground](http://www.documentdb.com/sql/demo) där du kan prova Azure Cosmos DB och köra SQL-frågor mot vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="f7992-107">We also invite you to visit the [Query Playground](http://www.documentdb.com/sql/demo) where you can try Azure Cosmos DB and run SQL queries against our dataset.</span></span>  
  
## <a name="select-query"></a><span data-ttu-id="f7992-108">SELECT-frågan</span><span class="sxs-lookup"><span data-stu-id="f7992-108">SELECT query</span></span>  
<span data-ttu-id="f7992-109">Hämtar JSON-dokument från databasen.</span><span class="sxs-lookup"><span data-stu-id="f7992-109">Retrieves JSON documents from the database.</span></span> <span data-ttu-id="f7992-110">Stöder uttrycksutvärdering, projektioner filtrering och ansluter till.</span><span class="sxs-lookup"><span data-stu-id="f7992-110">Supports expression evaluation, projections, filtering and joins.</span></span>  <span data-ttu-id="f7992-111">Konventioner som används för att beskriva SELECT-satser visas som en tabell i avsnittet Syntax konventioner.</span><span class="sxs-lookup"><span data-stu-id="f7992-111">The conventions used for describing the SELECT statements are tabulated in the Syntax conventions section.</span></span>  
  
<span data-ttu-id="f7992-112">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-112">**Syntax**</span></span>  
  
```
<select_query> ::=  
SELECT <select_specification>   
    [ FROM <from_specification>]   
    [ WHERE <filter_condition> ]  
    [ ORDER BY <sort_specification> ]  
```  
  
 <span data-ttu-id="f7992-113">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-113">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-114">Se följande avsnitt för mer information om varje sats:</span><span class="sxs-lookup"><span data-stu-id="f7992-114">See following sections for details on each clause:</span></span>  
  
-   [<span data-ttu-id="f7992-115">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-115">SELECT clause</span></span>](#bk_select_query)  
  
-   [<span data-ttu-id="f7992-116">FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-116">FROM clause</span></span>](#bk_from_clause)  
  
-   [<span data-ttu-id="f7992-117">WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-117">WHERE clause</span></span>](#bk_where_clause)  
  
-   [<span data-ttu-id="f7992-118">ORDER BY-sats</span><span class="sxs-lookup"><span data-stu-id="f7992-118">ORDER BY clause</span></span>](#bk_orderby_clause)  
  
<span data-ttu-id="f7992-119">Satser i SELECT-instruktionen måste beställas som ovan.</span><span class="sxs-lookup"><span data-stu-id="f7992-119">The clauses in the SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="f7992-120">En av de valfria satserna kan utelämnas.</span><span class="sxs-lookup"><span data-stu-id="f7992-120">Any one of the optional clauses can be omitted.</span></span> <span data-ttu-id="f7992-121">Men när valfria satser används, måste de visas i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="f7992-121">But when optional clauses are used, they must appear in the right order.</span></span>  
  
<span data-ttu-id="f7992-122">**Logiska behandlingsordning SELECT-instruktionen**</span><span class="sxs-lookup"><span data-stu-id="f7992-122">**Logical Processing Order of the SELECT statement**</span></span>  
  
<span data-ttu-id="f7992-123">Den ordning i vilken satser bearbetas är:</span><span class="sxs-lookup"><span data-stu-id="f7992-123">The order in which clauses are processed is:</span></span>  

1.  [<span data-ttu-id="f7992-124">FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-124">FROM clause</span></span>](#bk_from_clause)  
2.  [<span data-ttu-id="f7992-125">WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-125">WHERE clause</span></span>](#bk_where_clause)  
3.  [<span data-ttu-id="f7992-126">ORDER BY-sats</span><span class="sxs-lookup"><span data-stu-id="f7992-126">ORDER BY clause</span></span>](#bk_orderby_clause)  
4.  [<span data-ttu-id="f7992-127">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-127">SELECT clause</span></span>](#bk_select_query)  

<span data-ttu-id="f7992-128">Observera att detta skiljer sig från den ordning som de visas i syntax.</span><span class="sxs-lookup"><span data-stu-id="f7992-128">Note that this is different from the order in which they appear in the syntax.</span></span> <span data-ttu-id="f7992-129">Ordningsföljden är så att alla nya symboler som introducerades av en bearbetade sats visas och kan användas i satser bearbetas senare.</span><span class="sxs-lookup"><span data-stu-id="f7992-129">The ordering is such that all new symbols introduced by a processed clause are visible and can be used in clauses processed later.</span></span> <span data-ttu-id="f7992-130">Till exempel alias som deklareras i en FROM-sats är tillgängliga i var och SELECT-satser.</span><span class="sxs-lookup"><span data-stu-id="f7992-130">For instance, aliases declared in a FROM clause are accessible in WHERE and SELECT clauses.</span></span>  

<span data-ttu-id="f7992-131">**Tecken som blanksteg och kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-131">**Whitespace characters and comments**</span></span>  

<span data-ttu-id="f7992-132">Alla blanktecken som inte är del av en sträng inom citattecken eller identifierare med citattecken ingår inte i grammatiken språk och ignoreras vid parsning.</span><span class="sxs-lookup"><span data-stu-id="f7992-132">All white space characters which are not part of a quoted string or quoted identifier are not part of the language grammar and are ignored during parsing.</span></span>  

<span data-ttu-id="f7992-133">Kommentarer för T-SQL-format som har stöd för frågespråket</span><span class="sxs-lookup"><span data-stu-id="f7992-133">The query language supports T-SQL style comments like</span></span>  

-   <span data-ttu-id="f7992-134">SQL-uttryck`-- comment text [newline]`</span><span class="sxs-lookup"><span data-stu-id="f7992-134">SQL Statement `-- comment text [newline]`</span></span>  

<span data-ttu-id="f7992-135">Medan tecken som blanksteg och kommentarer inte har någon betydelse i grammatik, måste de användas för att avgränsa token.</span><span class="sxs-lookup"><span data-stu-id="f7992-135">While whitespace characters and comments do not have any significance in the grammar, they must be used to separate tokens.</span></span> <span data-ttu-id="f7992-136">Exempel: `-1e5` är ett enda nummer token, tag`: – 1 e5` följs minus token av nummer 1 och identifierare e5.</span><span class="sxs-lookup"><span data-stu-id="f7992-136">For instance: `-1e5` is a single number token, while`: – 1 e5` is a minus token followed by number 1 and identifier e5.</span></span>  

##  <span data-ttu-id="f7992-137"><a name="bk_select_query"></a>SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-137"><a name="bk_select_query"></a> SELECT clause</span></span>  
<span data-ttu-id="f7992-138">Satser i SELECT-instruktionen måste beställas som ovan.</span><span class="sxs-lookup"><span data-stu-id="f7992-138">The clauses in the SELECT statement must be ordered as shown above.</span></span> <span data-ttu-id="f7992-139">En av de valfria satserna kan utelämnas.</span><span class="sxs-lookup"><span data-stu-id="f7992-139">Any one of the optional clauses can be omitted.</span></span> <span data-ttu-id="f7992-140">Men när valfria satser används, måste de visas i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="f7992-140">But when optional clauses are used, they must appear in the right order.</span></span>  

<span data-ttu-id="f7992-141">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-141">**Syntax**</span></span>  
```  
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | <object_property_list>   
      | VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
 <span data-ttu-id="f7992-142">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-142">**Arguments**</span></span>  
  
 `<select_specification>`  
  
 <span data-ttu-id="f7992-143">Egenskaper eller -värde som ska väljas för resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-143">Properties or value to be selected for the result set.</span></span>  
  
 `'*'`  
  
<span data-ttu-id="f7992-144">Anger att värdet ska hämtas utan ändringar.</span><span class="sxs-lookup"><span data-stu-id="f7992-144">Specifies that the value should be retrieved without making any changes.</span></span> <span data-ttu-id="f7992-145">Mer specifikt om bearbetade värdet är ett objekt, hämtas alla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f7992-145">Specifically if the processed value is an object, all properties will be retrieved.</span></span>  
  
 `<object_property_list>`  
  
<span data-ttu-id="f7992-146">Anger en lista över egenskaper som ska hämtas.</span><span class="sxs-lookup"><span data-stu-id="f7992-146">Specifies the list of properties to be retrieved.</span></span> <span data-ttu-id="f7992-147">Varje returnerade värdet ska vara ett objekt med egenskaper som anges.</span><span class="sxs-lookup"><span data-stu-id="f7992-147">Each returned value will be an object with the properties specified.</span></span>  
  
`VALUE`  
  
<span data-ttu-id="f7992-148">Anger att JSON-värde ska hämtas i stället för fullständig JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-148">Specifies that the JSON value should be retrieved instead of the complete JSON object.</span></span> <span data-ttu-id="f7992-149">Detta, till skillnad från `<property_list>` radbryts inte planerade värdet i ett objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-149">This, unlike `<property_list>` does not wrap the projected value in an object.</span></span>  
  
`<scalar_expression>`  
  
<span data-ttu-id="f7992-150">Uttryck som representerar värdet ska beräknas.</span><span class="sxs-lookup"><span data-stu-id="f7992-150">Expression representing the value to be computed.</span></span> <span data-ttu-id="f7992-151">Se [skaläruttryck](#bk_scalar_expressions) information.</span><span class="sxs-lookup"><span data-stu-id="f7992-151">See [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
<span data-ttu-id="f7992-152">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-152">**Remarks**</span></span>  
  
<span data-ttu-id="f7992-153">Den `SELECT *` syntax är bara giltigt om FROM-satsen har deklarerats exakt ett alias.</span><span class="sxs-lookup"><span data-stu-id="f7992-153">The `SELECT *` syntax is only valid if FROM clause has declared exactly one alias.</span></span> <span data-ttu-id="f7992-154">`SELECT *`innehåller en identity-projektion som kan vara användbar om det behövs ingen projektion.</span><span class="sxs-lookup"><span data-stu-id="f7992-154">`SELECT *` provides an identity projection, which can be useful if no projection is needed.</span></span> <span data-ttu-id="f7992-155">Välj * är bara giltigt om FROM-satsen har angetts och införs bara en enda Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="f7992-155">SELECT * is only valid if FROM clause is specified and introduced only a single input source.</span></span>  
  
<span data-ttu-id="f7992-156">Observera att `SELECT <select_list>` och `SELECT *` är ”syntaktiska socker” och kan uttryckas också med hjälp av enkla SELECT-satser som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="f7992-156">Note that `SELECT <select_list>` and `SELECT *` are "syntactic sugar" and can be alternatively expressed by using simple SELECT statements as shown below.</span></span>  
  
1.  `SELECT * FROM ... AS from_alias ...`  
  
     <span data-ttu-id="f7992-157">motsvarar:</span><span class="sxs-lookup"><span data-stu-id="f7992-157">is equivalent to:</span></span>  
  
     `SELECT from_alias FROM ... AS from_alias ...`  
  
2.  `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
     <span data-ttu-id="f7992-158">motsvarar:</span><span class="sxs-lookup"><span data-stu-id="f7992-158">is equivalent to:</span></span>  
  
     `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
<span data-ttu-id="f7992-159">**Se även**</span><span class="sxs-lookup"><span data-stu-id="f7992-159">**See Also**</span></span>  
  
[<span data-ttu-id="f7992-160">Skalära uttryck</span><span class="sxs-lookup"><span data-stu-id="f7992-160">Scalar expressions</span></span>](#bk_scalar_expressions)  
[<span data-ttu-id="f7992-161">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-161">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="f7992-162"><a name="bk_from_clause"></a>FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-162"><a name="bk_from_clause"></a> FROM clause</span></span>  
<span data-ttu-id="f7992-163">Anger källan eller anslutna källor.</span><span class="sxs-lookup"><span data-stu-id="f7992-163">Specifies the source or joined sources.</span></span> <span data-ttu-id="f7992-164">FROM-satsen är valfritt.</span><span class="sxs-lookup"><span data-stu-id="f7992-164">The FROM clause is optional.</span></span> <span data-ttu-id="f7992-165">Om inte angivna, andra satser körs fortfarande som om FROM-satsen som ett enskilt dokument.</span><span class="sxs-lookup"><span data-stu-id="f7992-165">If not specified, other clauses will still be executed as if FROM clause provided a single document.</span></span>  
  
<span data-ttu-id="f7992-166">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-166">**Syntax**</span></span>  
  
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
  
<span data-ttu-id="f7992-167">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-167">**Arguments**</span></span>  
  
`<from_source>`  
  
<span data-ttu-id="f7992-168">Anger en datakälla med eller utan ett alias.</span><span class="sxs-lookup"><span data-stu-id="f7992-168">Specifies a data source, with or without an alias.</span></span> <span data-ttu-id="f7992-169">Om alias inte anges kommer den härledas från den `<collection_expression>` med hjälp av följande regler:</span><span class="sxs-lookup"><span data-stu-id="f7992-169">If alias is not specified, it will be inferred from the `<collection_expression>` using following rules:</span></span>  
  
-   <span data-ttu-id="f7992-170">Om uttrycket är ett samlingsnamn, kommer samlingsnamn att användas som ett alias.</span><span class="sxs-lookup"><span data-stu-id="f7992-170">If the expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
-   <span data-ttu-id="f7992-171">Om uttrycket är `<collection_expression>`, och sedan property_name sedan property_name kommer att användas som ett alias.</span><span class="sxs-lookup"><span data-stu-id="f7992-171">If the expression is `<collection_expression>`, then property_name, then property_name will be used as an alias.</span></span> <span data-ttu-id="f7992-172">Om uttrycket är ett samlingsnamn, kommer samlingsnamn att användas som ett alias.</span><span class="sxs-lookup"><span data-stu-id="f7992-172">If the expression is a collection_name, then collection_name will be used as an alias.</span></span>  
  
<span data-ttu-id="f7992-173">SOM`input_alias`</span><span class="sxs-lookup"><span data-stu-id="f7992-173">AS `input_alias`</span></span>  
  
<span data-ttu-id="f7992-174">Anger att den `input_alias` är en uppsättning värden som returneras av underliggande samlingsuttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-174">Specifies that the `input_alias` is a set of values returned by the underlying collection expression.</span></span>  
 
<span data-ttu-id="f7992-175">`input_alias`I</span><span class="sxs-lookup"><span data-stu-id="f7992-175">`input_alias` IN</span></span>  
  
<span data-ttu-id="f7992-176">Anger att den `input_alias` bör vara en uppsättning värden som hämtas av iterera över alla matriselement i varje matrisen som returneras av underliggande samlingsuttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-176">Specifies that the `input_alias` should represent the set of values obtained by iterating over all array elements of each array returned by the underlying collection expression.</span></span> <span data-ttu-id="f7992-177">Alla värden som returneras av underliggande samling uttryck som inte är en objektmatris ignoreras.</span><span class="sxs-lookup"><span data-stu-id="f7992-177">Any value returned by underlying collection expression that is not an array is ignored.</span></span>  
  
`<collection_expression>`  
  
<span data-ttu-id="f7992-178">Anger samlingsuttrycket som används för att hämta dokumenten.</span><span class="sxs-lookup"><span data-stu-id="f7992-178">Specifies the collection expression to be used to retrieve the documents.</span></span>  
  
`ROOT`  
  
<span data-ttu-id="f7992-179">Anger att dokumentet ska hämtas från standardvärdet anslutna samling.</span><span class="sxs-lookup"><span data-stu-id="f7992-179">Specifies that document should be retrieved from the default, currently connected collection.</span></span>  
  
`collection_name`  
  
<span data-ttu-id="f7992-180">Anger att dokumentet ska hämtas från den angivna samlingen.</span><span class="sxs-lookup"><span data-stu-id="f7992-180">Specifies that document should be retrieved from the provided collection.</span></span> <span data-ttu-id="f7992-181">Namnet på samlingen måste matcha namnet på samlingen som är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="f7992-181">The name of the collection must match the name of the collection currently connected to.</span></span>  
  
`input_alias`  
  
<span data-ttu-id="f7992-182">Anger att dokumentet ska hämtas från den källa som definieras av det angivna aliaset.</span><span class="sxs-lookup"><span data-stu-id="f7992-182">Specifies that document should be retrieved from the other source defined by the provided alias.</span></span>  
  
`<collection_expression> '.' property_`  
  
<span data-ttu-id="f7992-183">Anger att dokumentet ska hämtas genom att öppna den `property_name` egenskapen eller array_index matriselement för alla dokument som hämtas av angivna samlingsuttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-183">Specifies that document should be retrieved by accessing the `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
`<collection_expression> '[' "property_name" | array_index ']'`  
  
<span data-ttu-id="f7992-184">Anger att dokumentet ska hämtas genom att öppna den `property_name` egenskapen eller array_index matriselement för alla dokument som hämtas av angivna samlingsuttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-184">Specifies that document should be retrieved by accessing the `property_name` property or array_index array element for all documents retrieved by specified collection expression.</span></span>  
  
<span data-ttu-id="f7992-185">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-185">**Remarks**</span></span>  
  
<span data-ttu-id="f7992-186">Alla alias tillhandahålls eller härleda i den `<from_source>(`s) måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="f7992-186">All aliases provided or inferred in the `<from_source>(`s) must be unique.</span></span> <span data-ttu-id="f7992-187">Syntaxen `<collection_expression>.`kubattributbindningen är samma som `<collection_expression>' ['"property_name"']'`.</span><span class="sxs-lookup"><span data-stu-id="f7992-187">The Syntax `<collection_expression>.`property_name is the same as `<collection_expression>' ['"property_name"']'`.</span></span> <span data-ttu-id="f7992-188">Senare syntaxen kan dock användas om ett egenskapsnamn som innehåller en icke-ID-tecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-188">However, the latter syntax can be used if a property name contains a non-identifier characters.</span></span>  
  
<span data-ttu-id="f7992-189">**Saknar egenskaper, saknas matriselement, Odefinierad värden hantering**</span><span class="sxs-lookup"><span data-stu-id="f7992-189">**Missing properties, missing array elements, undefined values handling**</span></span>  
  
<span data-ttu-id="f7992-190">Om ett uttryck för samlingen använder egenskaper eller array-element och att värdet inte finns kan ignoreras värdet och inte bearbetas ytterligare.</span><span class="sxs-lookup"><span data-stu-id="f7992-190">If a collection expression accesses properties or array elements and that value does not exist, that value will be ignored and not processed further.</span></span>  
  
<span data-ttu-id="f7992-191">**Omfattningen för samlingen uttryck kontext**</span><span class="sxs-lookup"><span data-stu-id="f7992-191">**Collection expression context scoping**</span></span>  
  
<span data-ttu-id="f7992-192">En samling uttryck kan vara samling omfång eller dokumentet omfattar:</span><span class="sxs-lookup"><span data-stu-id="f7992-192">A collection expression may be collection-scoped or document-scoped:</span></span>  
  
-   <span data-ttu-id="f7992-193">Ett uttryck är en samling-omfång, om den underliggande datakällan samlingsuttrycket är antingen rot eller `collection_name`.</span><span class="sxs-lookup"><span data-stu-id="f7992-193">An expression is collection-scoped, if the underlying source of the collection expression is either ROOT or `collection_name`.</span></span> <span data-ttu-id="f7992-194">Dessa uttryck representerar en uppsättning dokument som hämtats från samlingen direkt och är inte beroende av bearbetning av andra uttryck för samlingen.</span><span class="sxs-lookup"><span data-stu-id="f7992-194">Such an expression represents a set of documents retrieved from the collection directly, and is not dependent on the processing of other collection expressions.</span></span>  
  
-   <span data-ttu-id="f7992-195">Ett uttryck är dokument-omfång, om den underliggande datakällan samlingsuttrycket `input_alias` introduceras tidigare i frågan.</span><span class="sxs-lookup"><span data-stu-id="f7992-195">An expression is document-scoped, if the underlying source of the collection expression is `input_alias` introduced earlier in the query.</span></span> <span data-ttu-id="f7992-196">Dessa uttryck representerar en uppsättning dokument som erhålls genom att utvärdera samlingsuttrycket i omfånget för varje dokument som tillhör uppsättningen som är associerade med samlingen alias.</span><span class="sxs-lookup"><span data-stu-id="f7992-196">Such an expression represents a set of documents obtained by evaluating the collection expression in the scope of each document belonging to the set associated with the aliased collection.</span></span>  <span data-ttu-id="f7992-197">Den resulterande uppsättningen kommer att vara en union av mängder som erhålls genom att utvärdera samlingsuttrycket för varje dokument i den underliggande uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-197">The resulting set will be a union of sets obtained by evaluating the collection expression for each of the documents in the underlying set.</span></span>  
  
<span data-ttu-id="f7992-198">**Kopplingar**</span><span class="sxs-lookup"><span data-stu-id="f7992-198">**Joins**</span></span>  
  
<span data-ttu-id="f7992-199">I den aktuella versionen stöder Azure Cosmos DB inre kopplingar.</span><span class="sxs-lookup"><span data-stu-id="f7992-199">In the current release, Azure Cosmos DB supports inner joins.</span></span> <span data-ttu-id="f7992-200">Anslut till ytterligare funktioner är kommande.</span><span class="sxs-lookup"><span data-stu-id="f7992-200">Additional join capabilities are forthcoming.</span></span>

<span data-ttu-id="f7992-201">Inre kopplingar resultera i en fullständig kryssprodukten av mängderna deltar i kopplingen.</span><span class="sxs-lookup"><span data-stu-id="f7992-201">Inner joins result in a complete cross product of the sets participating in the join.</span></span> <span data-ttu-id="f7992-202">Resultatet av en N-vägs koppling är en uppsättning element N tupplar, där varje värde i tuppeln är associerad med alias som deltar i kopplingen och kan nås av refererar till detta alias i andra-satser.</span><span class="sxs-lookup"><span data-stu-id="f7992-202">The result of an N-way join is a set of N-element tuples, where each value in the tuple is associated with the aliased set participating in the join and can be accessed by referencing that alias in other clauses.</span></span>  
  
<span data-ttu-id="f7992-203">Utvärderingen av kopplingen beror på kontexten omfånget för deltagande anger:</span><span class="sxs-lookup"><span data-stu-id="f7992-203">The evaluation of the join depends on the context scope of the participating sets:</span></span>  
  
-  <span data-ttu-id="f7992-204">En koppling mellan samlingsuppsättningen A och samling omfång ange B, resulterar i en kryssprodukten för alla element i anger A och b</span><span class="sxs-lookup"><span data-stu-id="f7992-204">A join between collection-set A and collection-scoped set B, results in a cross product of all elements in sets A and B.</span></span>
  
-   <span data-ttu-id="f7992-205">En koppling mellan uppsättning A och dokumentet omfång B, som resulterar i en union av alla uppsättningar som erhålls genom att utvärdera dokument-omfattande uppsättning B för varje dokument från ange A.</span><span class="sxs-lookup"><span data-stu-id="f7992-205">A join between set A and document-scoped set B, results in a union of all sets obtained by evaluating document-scoped set B for each document from set A.</span></span>  
  
 <span data-ttu-id="f7992-206">Maximalt en samling omfång uttryck stöds i den aktuella versionen av frågeprocessorn.</span><span class="sxs-lookup"><span data-stu-id="f7992-206">In the current release, a maximum of one collection-scoped expression is supported by the query processor.</span></span>  
  
<span data-ttu-id="f7992-207">**Exempel på kopplingar:**</span><span class="sxs-lookup"><span data-stu-id="f7992-207">**Examples of joins:**</span></span>  
  
<span data-ttu-id="f7992-208">Nu ska vi titta på följande FROM-satsen:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span><span class="sxs-lookup"><span data-stu-id="f7992-208">Let's look at the following FROM clause: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`</span></span>  
  
 <span data-ttu-id="f7992-209">Låt varje källa definiera `input_alias1, input_alias2, …, input_aliasN`.</span><span class="sxs-lookup"><span data-stu-id="f7992-209">Let each source define `input_alias1, input_alias2, …, input_aliasN`.</span></span> <span data-ttu-id="f7992-210">FROM-satsen returnerar en mängd med N-tupplar (tuppel med N värden).</span><span class="sxs-lookup"><span data-stu-id="f7992-210">This FROM clause returns a set of N-tuples (tuple with N values).</span></span> <span data-ttu-id="f7992-211">Varje tuppel har värden som genereras av alla samling alias iterera över sina respektive uppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f7992-211">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span>  
  
<span data-ttu-id="f7992-212">*Gå med i exempel 1, med 2 källor:*</span><span class="sxs-lookup"><span data-stu-id="f7992-212">*JOIN example 1, with 2 sources:*</span></span>  
  
- <span data-ttu-id="f7992-213">Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="f7992-213">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="f7992-214">Låt `<from_source2>` vara dokumentet definitionsområde refererar till input_alias1 och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="f7992-214">Let `<from_source2>` be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="f7992-215">{1, 2} för`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="f7992-215">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="f7992-216">{3} för`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="f7992-216">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="f7992-217">{4, 5} för`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="f7992-217">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="f7992-218">FROM-satsen `<from_source1> JOIN <from_source2>` resulterar i följande tupplar:</span><span class="sxs-lookup"><span data-stu-id="f7992-218">The FROM clause `<from_source1> JOIN <from_source2>` will result in the following tuples:</span></span>  
  
    <span data-ttu-id="f7992-219">(`input_alias1, input_alias2`):</span><span class="sxs-lookup"><span data-stu-id="f7992-219">(`input_alias1, input_alias2`):</span></span>  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
<span data-ttu-id="f7992-220">*Ansluta till exempel 2, med 3 källor:*</span><span class="sxs-lookup"><span data-stu-id="f7992-220">*JOIN example 2, with 3 sources:*</span></span>  
  
- <span data-ttu-id="f7992-221">Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="f7992-221">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="f7992-222">Låt `<from_source2>` vara omfång dokumentet refererar till `input_alias1` och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="f7992-222">Let `<from_source2>` be document-scoped referencing `input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="f7992-223">{1, 2} för`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="f7992-223">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="f7992-224">{3} för`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="f7992-224">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="f7992-225">{4, 5} för`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="f7992-225">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="f7992-226">Låt `<from_source3>` vara omfång dokumentet refererar till `input_alias2` och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="f7992-226">Let `<from_source3>` be document-scoped referencing `input_alias2` and represent sets:</span></span>  
  
    <span data-ttu-id="f7992-227">{100, 200} för`input_alias2 = 1,`</span><span class="sxs-lookup"><span data-stu-id="f7992-227">{100, 200} for `input_alias2 = 1,`</span></span>  
  
    <span data-ttu-id="f7992-228">{300} för`input_alias2 = 3,`</span><span class="sxs-lookup"><span data-stu-id="f7992-228">{300} for `input_alias2 = 3,`</span></span>  
  
- <span data-ttu-id="f7992-229">FROM-satsen `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulterar i följande tupplar:</span><span class="sxs-lookup"><span data-stu-id="f7992-229">The FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in the following tuples:</span></span>  
  
    <span data-ttu-id="f7992-230">(input_alias1, input_alias2, input_alias3):</span><span class="sxs-lookup"><span data-stu-id="f7992-230">(input_alias1, input_alias2, input_alias3):</span></span>  
  
    <span data-ttu-id="f7992-231">A-, 1, 100 (A, 1, 200), (B, 3, 300)</span><span class="sxs-lookup"><span data-stu-id="f7992-231">(A, 1, 100), (A, 1, 200), (B, 3, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="f7992-232">Avsaknad av tupplar för andra `input_alias1`, `input_alias2`, för vilka den `<from_source3>` returnerade inte några värden.</span><span class="sxs-lookup"><span data-stu-id="f7992-232">Lack of tuples for other values of `input_alias1`, `input_alias2`, for which the `<from_source3>` did not return any values.</span></span>  
  
<span data-ttu-id="f7992-233">*Ansluta till exempel 3, med 3 källor:*</span><span class="sxs-lookup"><span data-stu-id="f7992-233">*JOIN example 3, with 3 sources:*</span></span>  
  
- <span data-ttu-id="f7992-234">Låt < from_source1 > vara begränsad samling som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="f7992-234">Let <from_source1> be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="f7992-235">Låt `<from_source1>` samling-begränsas som representerar uppsättningen {A, B, C}.</span><span class="sxs-lookup"><span data-stu-id="f7992-235">Let `<from_source1>` be collection-scoped and represent set {A, B, C}.</span></span>  
  
- <span data-ttu-id="f7992-236">Låt < from_source2 > att dokumentet omfång refererande input_alias1 och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="f7992-236">Let <from_source2> be document-scoped referencing input_alias1 and represent sets:</span></span>  
  
    <span data-ttu-id="f7992-237">{1, 2} för`input_alias1 = A,`</span><span class="sxs-lookup"><span data-stu-id="f7992-237">{1, 2} for `input_alias1 = A,`</span></span>  
  
    <span data-ttu-id="f7992-238">{3} för`input_alias1 = B,`</span><span class="sxs-lookup"><span data-stu-id="f7992-238">{3} for `input_alias1 = B,`</span></span>  
  
    <span data-ttu-id="f7992-239">{4, 5} för`input_alias1 = C,`</span><span class="sxs-lookup"><span data-stu-id="f7992-239">{4, 5} for `input_alias1 = C,`</span></span>  
  
- <span data-ttu-id="f7992-240">Låt `<from_source3>` vara begränsad till `input_alias1` och representerar anger:</span><span class="sxs-lookup"><span data-stu-id="f7992-240">Let `<from_source3>` be scoped to `input_alias1` and represent sets:</span></span>  
  
    <span data-ttu-id="f7992-241">{100, 200} för`input_alias2 = A,`</span><span class="sxs-lookup"><span data-stu-id="f7992-241">{100, 200} for `input_alias2 = A,`</span></span>  
  
    <span data-ttu-id="f7992-242">{300} för`input_alias2 = C,`</span><span class="sxs-lookup"><span data-stu-id="f7992-242">{300} for `input_alias2 = C,`</span></span>  
  
- <span data-ttu-id="f7992-243">FROM-satsen `<from_source1> JOIN <from_source2> JOIN <from_source3>` resulterar i följande tupplar:</span><span class="sxs-lookup"><span data-stu-id="f7992-243">The FROM clause `<from_source1> JOIN <from_source2> JOIN <from_source3>` will result in the following tuples:</span></span>  
  
    <span data-ttu-id="f7992-244">(`input_alias1, input_alias2, input_alias3`):</span><span class="sxs-lookup"><span data-stu-id="f7992-244">(`input_alias1, input_alias2, input_alias3`):</span></span>  
  
    <span data-ttu-id="f7992-245">A-, 1, 100 (A, 1, 200), A-, 2, 100 A-, 2, 200 C, 4, 300, (C, 5, 300)</span><span class="sxs-lookup"><span data-stu-id="f7992-245">(A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200),  (C, 4, 300) ,  (C, 5, 300)</span></span>  
  
> [!NOTE]
> <span data-ttu-id="f7992-246">Detta resulterade i kryssprodukten mellan `<from_source2>` och `<from_source3>` eftersom båda är begränsade till samma `<from_source1>`.</span><span class="sxs-lookup"><span data-stu-id="f7992-246">This resulted in cross product between `<from_source2>` and `<from_source3>` because both are scoped to the same `<from_source1>`.</span></span>  <span data-ttu-id="f7992-247">Detta resulterade i 4.2 2 x tupplar med värdet A, 0 tupplar med värdet B (1 x 0) och 2 (2 x 1) tupplar med värdet C.</span><span class="sxs-lookup"><span data-stu-id="f7992-247">This resulted in 4 (2x2) tuples having value A, 0 tuples having value B (1x0) and 2 (2x1) tuples having value C.</span></span>  
  
<span data-ttu-id="f7992-248">**Se även**</span><span class="sxs-lookup"><span data-stu-id="f7992-248">**See also**</span></span>  
  
 [<span data-ttu-id="f7992-249">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-249">SELECT clause</span></span>](#bk_select_query)  
  
##  <span data-ttu-id="f7992-250"><a name="bk_where_clause"></a>WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="f7992-250"><a name="bk_where_clause"></a> WHERE clause</span></span>  
 <span data-ttu-id="f7992-251">Anger sökvillkor för dokument som returneras av frågan.</span><span class="sxs-lookup"><span data-stu-id="f7992-251">Specifies the search condition for the documents returned by the query.</span></span>  
  
 <span data-ttu-id="f7992-252">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-252">**Syntax**</span></span>  
  
```  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
 <span data-ttu-id="f7992-253">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-253">**Arguments**</span></span>  
  
-   `<filter_condition>`  
  
     <span data-ttu-id="f7992-254">Anger villkor som måste uppfyllas för dokument som ska returneras.</span><span class="sxs-lookup"><span data-stu-id="f7992-254">Specifies the condition to be met for the documents to be returned.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="f7992-255">Uttryck som representerar värdet ska beräknas.</span><span class="sxs-lookup"><span data-stu-id="f7992-255">Expression representing the value to be computed.</span></span> <span data-ttu-id="f7992-256">Finns det [skaläruttryck](#bk_scalar_expressions) information.</span><span class="sxs-lookup"><span data-stu-id="f7992-256">See the [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
 <span data-ttu-id="f7992-257">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-257">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-258">Villkoret måste utvärderas till SANT för att dokumentet ska returneras ett uttryck har angetts som filter.</span><span class="sxs-lookup"><span data-stu-id="f7992-258">In order for the document to be returned an expression specified as filter condition must evaluate to true.</span></span> <span data-ttu-id="f7992-259">Endast booleska värdet true kommer uppfyller villkoret andra värden: Odefinierad, null, false, antalet, matris eller ett objekt ska inte uppfyller villkoret.</span><span class="sxs-lookup"><span data-stu-id="f7992-259">Only Boolean value true will satisfy the condition, any other value: undefined, null, false, Number, Array or Object will not satisfy the condition.</span></span>  
  
##  <span data-ttu-id="f7992-260"><a name="bk_orderby_clause"></a>ORDER BY-sats</span><span class="sxs-lookup"><span data-stu-id="f7992-260"><a name="bk_orderby_clause"></a> ORDER BY clause</span></span>  
 <span data-ttu-id="f7992-261">Anger sorteringsordning för resultaten som returnerades av frågan.</span><span class="sxs-lookup"><span data-stu-id="f7992-261">Specifies the sorting order for results returned by the query.</span></span>  
  
 <span data-ttu-id="f7992-262">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-262">**Syntax**</span></span>  
  
```  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= <scalar_expression> [ASC | DESC]  
  
```  
  
 <span data-ttu-id="f7992-263">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-263">**Arguments**</span></span>  
  
-   `<sort_specification>`  
  
     <span data-ttu-id="f7992-264">Anger en egenskap eller ett uttryck som du vill sortera frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="f7992-264">Specifies a property or expression on which to sort the query result set.</span></span> <span data-ttu-id="f7992-265">En sorteringskolumn kan anges som ett alias eller en kolumn.</span><span class="sxs-lookup"><span data-stu-id="f7992-265">A sort column can be specified as a name or column alias.</span></span>  
  
     <span data-ttu-id="f7992-266">Du kan ange flera sorteringskolumner.</span><span class="sxs-lookup"><span data-stu-id="f7992-266">Multiple sort columns can be specified.</span></span> <span data-ttu-id="f7992-267">Kolumnnamnen måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="f7992-267">Column names must be unique.</span></span> <span data-ttu-id="f7992-268">Sekvensen av sorteringskolumner i ORDER BY-satsen definierar organisationen för sorterade resultatmängden.</span><span class="sxs-lookup"><span data-stu-id="f7992-268">The sequence of the sort columns in the ORDER BY clause defines the organization of the sorted result set.</span></span> <span data-ttu-id="f7992-269">Det vill säga resultatet sorteras efter den första egenskapen och sedan den beställda listan sorteras efter den andra egenskapen och så vidare.</span><span class="sxs-lookup"><span data-stu-id="f7992-269">That is, the result set is sorted by the first property and then that ordered list is sorted by the second property, and so on.</span></span>  
  
     <span data-ttu-id="f7992-270">Kolumnnamnen som refereras i ORDER BY-satsen måste matcha till antingen en kolumn i select-listan eller en kolumn som definierats i en tabell som har angetts i FROM-sats utan någon tvetydigheter.</span><span class="sxs-lookup"><span data-stu-id="f7992-270">The column names referenced in the ORDER BY clause must correspond to either a column in the select list or to a column defined in a table specified in the FROM clause without any ambiguities.</span></span>  
  
-   `<sort_expression>`  
  
     <span data-ttu-id="f7992-271">Anger en enskild egenskap eller ett uttryck som du vill sortera frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="f7992-271">Specifies a single property or expression on which to sort the query result set.</span></span>  
  
-   `<scalar_expression>`  
  
     <span data-ttu-id="f7992-272">Finns det [skaläruttryck](#bk_scalar_expressions) information.</span><span class="sxs-lookup"><span data-stu-id="f7992-272">See the [Scalar expressions](#bk_scalar_expressions) section for details.</span></span>  
  
-   `ASC | DESC`  
  
     <span data-ttu-id="f7992-273">Anger att värdena i den angivna kolumnen ska sorteras i stigande eller fallande ordning.</span><span class="sxs-lookup"><span data-stu-id="f7992-273">Specifies that the values in the specified column should be sorted in ascending or descending order.</span></span> <span data-ttu-id="f7992-274">ASC sorterar från det lägsta värdet för högsta värdet.</span><span class="sxs-lookup"><span data-stu-id="f7992-274">ASC sorts from the lowest value to highest value.</span></span> <span data-ttu-id="f7992-275">DESC sorterar från högsta till lägsta värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-275">DESC sorts from highest value to lowest value.</span></span> <span data-ttu-id="f7992-276">ASC är standardsorteringsordning.</span><span class="sxs-lookup"><span data-stu-id="f7992-276">ASC is the default sort order.</span></span> <span data-ttu-id="f7992-277">Null-värden behandlas som lägsta möjliga värden.</span><span class="sxs-lookup"><span data-stu-id="f7992-277">Null values are treated as the lowest possible values.</span></span>  
  
 <span data-ttu-id="f7992-278">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-278">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-279">Medan frågegrammatik stöder flera ordning efter egenskaper, stöder Azure Cosmos DB frågan runtime sortering bara mot en enskild egenskap och bara mot egenskapsnamn, d.v.s. inte mot beräknade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f7992-279">While the query grammar supports multiple order by properties, the Azure Cosmos DB query runtime supports sorting only against a single property, and only against property names, i.e., not against computed properties.</span></span> <span data-ttu-id="f7992-280">Sortering kräver också att indexprincip innehåller ett index för intervallet för egenskapen och den angivna typen med den maximala precisionen.</span><span class="sxs-lookup"><span data-stu-id="f7992-280">Sorting also requires that the indexing policy includes a range index for the property and the specified type, with the maximum precision.</span></span> <span data-ttu-id="f7992-281">Referera till indexeringstjänsten princip dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="f7992-281">Refer to the indexing policy documentation for more details.</span></span>  
  
##  <span data-ttu-id="f7992-282"><a name="bk_scalar_expressions"></a>Skalära uttryck</span><span class="sxs-lookup"><span data-stu-id="f7992-282"><a name="bk_scalar_expressions"></a> Scalar expressions</span></span>  
 <span data-ttu-id="f7992-283">Ett skalärt uttryck som är en kombination av symboler och operatorer som kan utvärderas för att få ett enskilt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-283">A scalar expression is a combination of symbols and operators that can be evaluated to obtain a single value.</span></span> <span data-ttu-id="f7992-284">Enkla uttryck kan vara konstanter, egenskapsreferenser, matris referenser, alias referenser eller funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="f7992-284">Simple expressions can be constants, property references, array element references, alias references, or function calls.</span></span> <span data-ttu-id="f7992-285">Enkla uttryck kan kombineras till komplexa uttryck med hjälp av operatörer.</span><span class="sxs-lookup"><span data-stu-id="f7992-285">Simple expressions can be combined into complex expressions using operators.</span></span>  
  
 <span data-ttu-id="f7992-286">Mer information om vilka skalärt uttryck som kan ha värden finns [konstanter](#bk_constants) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f7992-286">For details on values which scalar expression may have, see [Constants](#bk_constants) section.</span></span>  
  
 <span data-ttu-id="f7992-287">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-287">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="f7992-288">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-288">**Arguments**</span></span>  
  
-   `<constant>`  
  
     <span data-ttu-id="f7992-289">Representerar ett konstant värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-289">Represents a constant value.</span></span> <span data-ttu-id="f7992-290">Se [konstanter](#bk_constants) information.</span><span class="sxs-lookup"><span data-stu-id="f7992-290">See [Constants](#bk_constants) section for details.</span></span>  
  
-   `input_alias`  
  
     <span data-ttu-id="f7992-291">Representerar ett värde som definieras av den `input_alias` introducerades i den `FROM` satsen.</span><span class="sxs-lookup"><span data-stu-id="f7992-291">Represents a value defined by the `input_alias` introduced in the `FROM` clause.</span></span>  
    <span data-ttu-id="f7992-292">Det här värdet är garanterat inte **Odefinierad** –**Odefinierad** värden i inkommande hoppas över.</span><span class="sxs-lookup"><span data-stu-id="f7992-292">This value is guaranteed to not be **undefined** –**undefined** values in the input are skipped.</span></span>  
  
-   `<scalar_expression>.property_name`  
  
     <span data-ttu-id="f7992-293">Representerar ett värde för egenskapen för ett objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-293">Represents a value of the property of an object.</span></span> <span data-ttu-id="f7992-294">Om egenskapen finns inte eller egenskapen refererar till ett värde som inte är ett objekt och sedan uttrycket utvärderas till **Odefinierad** värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-294">If the property does not exist or property is referenced on a value which is not an object, then the expression evaluates to **undefined** value.</span></span>  
  
-   `<scalar_expression>'['"property_name"|array_index']'`  
  
     <span data-ttu-id="f7992-295">Representerar ett värde för egenskapen med namnet `property_name` eller array-elementet med index `array_index` av en objektmatris.</span><span class="sxs-lookup"><span data-stu-id="f7992-295">Represents a value of the property with name `property_name` or array element with index `array_index` of an object/array.</span></span> <span data-ttu-id="f7992-296">Om egenskapen/matrisindexet finns inte eller egenskapen/matrisindexet refererar till ett värde som inte är en objektmatris, utvärderar uttrycket till odefinierat värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-296">If the property/array index does not exist or the property/array index is referenced on a value which is not an object/array, then the expression evaluates to undefined value.</span></span>  
  
-   `unary_operator <scalar_expression>`  
  
     <span data-ttu-id="f7992-297">Representerar en operator som används för ett enskilt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-297">Represents an operator that is applied to a single value.</span></span> <span data-ttu-id="f7992-298">Se [operatörer](#bk_operators) information.</span><span class="sxs-lookup"><span data-stu-id="f7992-298">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_expression> binary_operator <scalar_expression>`  
  
     <span data-ttu-id="f7992-299">Representerar en operator som används för två värden.</span><span class="sxs-lookup"><span data-stu-id="f7992-299">Represents an operator that is applied to two values.</span></span> <span data-ttu-id="f7992-300">Se [operatörer](#bk_operators) information.</span><span class="sxs-lookup"><span data-stu-id="f7992-300">See [Operators](#bk_operators) section for details.</span></span>  
  
-   `<scalar_function_expression>`  
  
     <span data-ttu-id="f7992-301">Representerar ett värde som definieras av ett resultat av ett funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="f7992-301">Represents a value defined by a result of a function call.</span></span>  
  
-   `udf_scalar_function`  
  
     <span data-ttu-id="f7992-302">Namnet på användaren definierat skalärfunktion.</span><span class="sxs-lookup"><span data-stu-id="f7992-302">Name of the user defined scalar function.</span></span>  
  
-   `builtin_scalar_function`  
  
     <span data-ttu-id="f7992-303">Namnet på den inbyggda skalärfunktion.</span><span class="sxs-lookup"><span data-stu-id="f7992-303">Name of the built-in scalar function.</span></span>  
  
-   `<create_object_expression>`  
  
     <span data-ttu-id="f7992-304">Representerar ett värde som erhålls genom att skapa ett nytt objekt med angivna egenskaper och deras värden.</span><span class="sxs-lookup"><span data-stu-id="f7992-304">Represents a value obtained by creating a new object with specified properties and their values.</span></span>  
  
-   `<create_array_expression>`  
  
     <span data-ttu-id="f7992-305">Representerar ett värde som erhålls genom att skapa en ny matris med angivna värden som element</span><span class="sxs-lookup"><span data-stu-id="f7992-305">Represents a value obtained by creating a new array with specified values as elements</span></span>  
  
-   `parameter_name`  
  
     <span data-ttu-id="f7992-306">Representerar ett värde för det angivna parameternamnet.</span><span class="sxs-lookup"><span data-stu-id="f7992-306">Represents a value of the specified parameter name.</span></span> <span data-ttu-id="f7992-307">Parameternamn måste ha en enda @ som första tecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-307">Parameter names must have a single @ as the first character.</span></span>  
  
 <span data-ttu-id="f7992-308">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-308">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-309">När du anropar en inbyggd eller användaren definierat skalärfunktion måste alla argument anges.</span><span class="sxs-lookup"><span data-stu-id="f7992-309">When calling a built-in or user defined scalar function all arguments must be defined.</span></span> <span data-ttu-id="f7992-310">Om något av argumenten är odefinierad funktionen kommer inte att avbrytas och resultatet blir odefinierad.</span><span class="sxs-lookup"><span data-stu-id="f7992-310">If any of the arguments is undefined, the function will not be called and the result will be undefined.</span></span>  
  
 <span data-ttu-id="f7992-311">När du skapar ett objekt ska hoppas över egenskaper som är tilldelad odefinierat värde och inte ingår i det skapade objektet.</span><span class="sxs-lookup"><span data-stu-id="f7992-311">When creating an object, any property that is assigned undefined value will be skipped and not included in the created object.</span></span>  
  
 <span data-ttu-id="f7992-312">När du skapar en matris, ett elementvärde som är tilldelad **Odefinierad** värdet kommer att hoppas över och inte ingår i det skapade objektet.</span><span class="sxs-lookup"><span data-stu-id="f7992-312">When creating an array, any element value that is assigned **undefined** value will be skipped and not included in the created object.</span></span> <span data-ttu-id="f7992-313">Detta innebär att nästa definierade element ska utföras så att den skapade matrisen inte kommer ha hoppas över index.</span><span class="sxs-lookup"><span data-stu-id="f7992-313">This will cause the next defined element to take its place in such a way that the created array will not have skipped indexes.</span></span>  
  
##  <span data-ttu-id="f7992-314"><a name="bk_operators"></a>Operatörer</span><span class="sxs-lookup"><span data-stu-id="f7992-314"><a name="bk_operators"></a> Operators</span></span>  
 <span data-ttu-id="f7992-315">Det här avsnittet beskrivs operatorerna som stöds.</span><span class="sxs-lookup"><span data-stu-id="f7992-315">This section describes the supported operators.</span></span> <span data-ttu-id="f7992-316">Varje operatör kan tilldelas till exakt en kategori.</span><span class="sxs-lookup"><span data-stu-id="f7992-316">Each operator can be assigned to exactly one category.</span></span>  
  
 <span data-ttu-id="f7992-317">Se **operatorn kategorier** tabellen nedan, mer information om hantering av **Odefinierad** värden, krav för indatavärden för och hanteringen av värden med typer som inte matchar.</span><span class="sxs-lookup"><span data-stu-id="f7992-317">See **Operator categories** table below, for details regarding handling of **undefined** values, type requirements for input values and handling of values with not matching types.</span></span>  
  
 <span data-ttu-id="f7992-318">**Operatorn kategorier:**</span><span class="sxs-lookup"><span data-stu-id="f7992-318">**Operator categories:**</span></span>  
  
|<span data-ttu-id="f7992-319">**Kategori**</span><span class="sxs-lookup"><span data-stu-id="f7992-319">**Category**</span></span>|<span data-ttu-id="f7992-320">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="f7992-320">**Details**</span></span>|  
|-|-|  
|<span data-ttu-id="f7992-321">**aritmetiska**</span><span class="sxs-lookup"><span data-stu-id="f7992-321">**arithmetic**</span></span>|<span data-ttu-id="f7992-322">Operatorn förväntar input(s) vara nummer.</span><span class="sxs-lookup"><span data-stu-id="f7992-322">Operator expects input(s) to be Number(s).</span></span> <span data-ttu-id="f7992-323">Utdata är också ett tal.</span><span class="sxs-lookup"><span data-stu-id="f7992-323">Output is also a Number.</span></span> <span data-ttu-id="f7992-324">Om någon av indata är **Odefinierad** eller annan typ än antalet sedan resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="f7992-324">If any of the inputs is **undefined** or type other than Number then the result is **undefined**.</span></span>|  
|<span data-ttu-id="f7992-325">**binär**</span><span class="sxs-lookup"><span data-stu-id="f7992-325">**bitwise**</span></span>|<span data-ttu-id="f7992-326">Operatorn förväntar input(s) vara 32-bitars heltal nummer.</span><span class="sxs-lookup"><span data-stu-id="f7992-326">Operator expects input(s) to be 32-bit signed integer Number(s).</span></span> <span data-ttu-id="f7992-327">Utdata är också 32-bitars heltal nummer.</span><span class="sxs-lookup"><span data-stu-id="f7992-327">Output is also 32-bit signed integer Number.</span></span><br /><br /> <span data-ttu-id="f7992-328">Att kommer avrundas valfritt heltal.</span><span class="sxs-lookup"><span data-stu-id="f7992-328">Any non-integer value will be rounded.</span></span> <span data-ttu-id="f7992-329">Ett positivt värde avrundas nedåt, negativa värden avrunda uppåt.</span><span class="sxs-lookup"><span data-stu-id="f7992-329">Positive value will be rounded down, negative values rounded up.</span></span><br /><br /> <span data-ttu-id="f7992-330">Ett värde som är utanför intervallet för 32-bitars heltal konverteras med sista 32-bitar för dess två visas.</span><span class="sxs-lookup"><span data-stu-id="f7992-330">Any value that is outside of the 32-bit integer range will be converted, by taking last 32-bits of its two's complement notation.</span></span><br /><br /> <span data-ttu-id="f7992-331">Om någon av indata är **Odefinierad** eller annan typ än siffra, och sedan resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="f7992-331">If any of the inputs is **undefined** or type other than Number, then the result is **undefined**.</span></span><br /><br /> <span data-ttu-id="f7992-332">**Obs:** beteendet ovan är kompatibel med JavaScript binär operator-beteende.</span><span class="sxs-lookup"><span data-stu-id="f7992-332">**Note:** The above behavior is compatible with JavaScript bitwise operator behavior.</span></span>|  
|<span data-ttu-id="f7992-333">**logiska**</span><span class="sxs-lookup"><span data-stu-id="f7992-333">**logical**</span></span>|<span data-ttu-id="f7992-334">Operatorn förväntar input(s) ska Boolean(s).</span><span class="sxs-lookup"><span data-stu-id="f7992-334">Operator expects input(s) to be Boolean(s).</span></span> <span data-ttu-id="f7992-335">Utdata är också ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-335">Output is also a Boolean.</span></span><br /><span data-ttu-id="f7992-336">Om någon av indata är **Odefinierad** eller annan typ än Boolean, och sedan resultatet blir **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="f7992-336">If any of the inputs is **undefined** or type other than Boolean, then the result will be **undefined**.</span></span>|  
|<span data-ttu-id="f7992-337">**jämförelse**</span><span class="sxs-lookup"><span data-stu-id="f7992-337">**comparison**</span></span>|<span data-ttu-id="f7992-338">Operatorn förväntar input(s) har samma typ och inte vara odefinierat.</span><span class="sxs-lookup"><span data-stu-id="f7992-338">Operator expects input(s) to have the same type and not be undefined.</span></span> <span data-ttu-id="f7992-339">Utdata är ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-339">Output is a Boolean.</span></span><br /><br /> <span data-ttu-id="f7992-340">Om någon av indata är **Odefinierad** eller indata har olika typer och sedan resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="f7992-340">If any of the inputs is **undefined** or the inputs have different types, then the result is **undefined**.</span></span><br /><br /> <span data-ttu-id="f7992-341">Se **ordning med värden för jämförelse** tabellen för värdet ordning information.</span><span class="sxs-lookup"><span data-stu-id="f7992-341">See **Ordering of values for comparison** table for value ordering details.</span></span>|  
|<span data-ttu-id="f7992-342">**sträng**</span><span class="sxs-lookup"><span data-stu-id="f7992-342">**string**</span></span>|<span data-ttu-id="f7992-343">Operatorn förväntar input(s) ska strängarna.</span><span class="sxs-lookup"><span data-stu-id="f7992-343">Operator expects input(s) to be String(s).</span></span> <span data-ttu-id="f7992-344">Utdata är också en sträng.</span><span class="sxs-lookup"><span data-stu-id="f7992-344">Output is also a String.</span></span><br /><span data-ttu-id="f7992-345">Om någon av indata är **Odefinierad** eller annan typ än sträng sedan resultatet är **Odefinierad**.</span><span class="sxs-lookup"><span data-stu-id="f7992-345">If any of the inputs is **undefined** or type other than String then the result is **undefined**.</span></span>|  
  
 <span data-ttu-id="f7992-346">**Unära operatorer:**</span><span class="sxs-lookup"><span data-stu-id="f7992-346">**Unary operators:**</span></span>  
  
|<span data-ttu-id="f7992-347">**Namn**</span><span class="sxs-lookup"><span data-stu-id="f7992-347">**Name**</span></span>|<span data-ttu-id="f7992-348">**Operatorn**</span><span class="sxs-lookup"><span data-stu-id="f7992-348">**Operator**</span></span>|<span data-ttu-id="f7992-349">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="f7992-349">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="f7992-350">**aritmetiska**</span><span class="sxs-lookup"><span data-stu-id="f7992-350">**arithmetic**</span></span>|+<br /><br /> -|<span data-ttu-id="f7992-351">Returnerar det numeriska värdet.</span><span class="sxs-lookup"><span data-stu-id="f7992-351">Returns the number value.</span></span><br /><br /> <span data-ttu-id="f7992-352">Binär negation.</span><span class="sxs-lookup"><span data-stu-id="f7992-352">Bitwise negation.</span></span> <span data-ttu-id="f7992-353">Returnerar negated numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-353">Returns negated number value.</span></span>|  
|<span data-ttu-id="f7992-354">**binär**</span><span class="sxs-lookup"><span data-stu-id="f7992-354">**bitwise**</span></span>|~|<span data-ttu-id="f7992-355">De som har komplementet.</span><span class="sxs-lookup"><span data-stu-id="f7992-355">Ones' complement.</span></span> <span data-ttu-id="f7992-356">Returnerar en uppsättning ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-356">Returns a complement of a number value.</span></span>|  
|<span data-ttu-id="f7992-357">**Logiska**</span><span class="sxs-lookup"><span data-stu-id="f7992-357">**Logical**</span></span>|<span data-ttu-id="f7992-358">**INTE**</span><span class="sxs-lookup"><span data-stu-id="f7992-358">**NOT**</span></span>|<span data-ttu-id="f7992-359">Negation.</span><span class="sxs-lookup"><span data-stu-id="f7992-359">Negation.</span></span> <span data-ttu-id="f7992-360">Returnerar negated booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-360">Returns negated Boolean value.</span></span>|  
  
 <span data-ttu-id="f7992-361">**Binära operatorer:**</span><span class="sxs-lookup"><span data-stu-id="f7992-361">**Binary operators:**</span></span>  
  
|<span data-ttu-id="f7992-362">**Namn**</span><span class="sxs-lookup"><span data-stu-id="f7992-362">**Name**</span></span>|<span data-ttu-id="f7992-363">**Operatorn**</span><span class="sxs-lookup"><span data-stu-id="f7992-363">**Operator**</span></span>|<span data-ttu-id="f7992-364">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="f7992-364">**Details**</span></span>|  
|-|-|-|  
|<span data-ttu-id="f7992-365">**aritmetiska**</span><span class="sxs-lookup"><span data-stu-id="f7992-365">**arithmetic**</span></span>|+<br /><br /> -<br /><br /> *<br /><br /> /<br /><br /> %|<span data-ttu-id="f7992-366">Tillägg.</span><span class="sxs-lookup"><span data-stu-id="f7992-366">Addition.</span></span><br /><br /> <span data-ttu-id="f7992-367">Subtraktion.</span><span class="sxs-lookup"><span data-stu-id="f7992-367">Subtraction.</span></span><br /><br /> <span data-ttu-id="f7992-368">Multiplikation.</span><span class="sxs-lookup"><span data-stu-id="f7992-368">Multiplication.</span></span><br /><br /> <span data-ttu-id="f7992-369">Division.</span><span class="sxs-lookup"><span data-stu-id="f7992-369">Division.</span></span><br /><br /> <span data-ttu-id="f7992-370">Modulering.</span><span class="sxs-lookup"><span data-stu-id="f7992-370">Modulation.</span></span>|  
|<span data-ttu-id="f7992-371">**binär**</span><span class="sxs-lookup"><span data-stu-id="f7992-371">**bitwise**</span></span>|<span data-ttu-id="f7992-372">&#124;</span><span class="sxs-lookup"><span data-stu-id="f7992-372">&#124;</span></span><br /><br /> &<br /><br /> ^<br /><br /> <<<br /><br /> >><br /><br /> >>>|<span data-ttu-id="f7992-373">Logiskt eller.</span><span class="sxs-lookup"><span data-stu-id="f7992-373">Bitwise OR.</span></span><br /><br /> <span data-ttu-id="f7992-374">Binär och.</span><span class="sxs-lookup"><span data-stu-id="f7992-374">Bitwise AND.</span></span><br /><br /> <span data-ttu-id="f7992-375">Bitvis XOR.</span><span class="sxs-lookup"><span data-stu-id="f7992-375">Bitwise XOR.</span></span><br /><br /> <span data-ttu-id="f7992-376">Vänsterskift.</span><span class="sxs-lookup"><span data-stu-id="f7992-376">Left Shift.</span></span><br /><br /> <span data-ttu-id="f7992-377">Högerskift.</span><span class="sxs-lookup"><span data-stu-id="f7992-377">Right Shift.</span></span><br /><br /> <span data-ttu-id="f7992-378">Noll fill högerskift.</span><span class="sxs-lookup"><span data-stu-id="f7992-378">Zero-fill Right Shift.</span></span>|  
|<span data-ttu-id="f7992-379">**logiska**</span><span class="sxs-lookup"><span data-stu-id="f7992-379">**logical**</span></span>|<span data-ttu-id="f7992-380">**OCH**</span><span class="sxs-lookup"><span data-stu-id="f7992-380">**AND**</span></span><br /><br /> <span data-ttu-id="f7992-381">**ELLER**</span><span class="sxs-lookup"><span data-stu-id="f7992-381">**OR**</span></span>|<span data-ttu-id="f7992-382">Logisk konjunktion.</span><span class="sxs-lookup"><span data-stu-id="f7992-382">Logical conjunction.</span></span> <span data-ttu-id="f7992-383">Returnerar **SANT** om båda argument är **SANT**, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-383">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="f7992-384">Logisk konjunktion.</span><span class="sxs-lookup"><span data-stu-id="f7992-384">Logical conjunction.</span></span> <span data-ttu-id="f7992-385">Returnerar **SANT** om båda argument är **SANT**, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-385">Returns **true** if both arguments are **true**, returns **false** otherwise.</span></span>|  
|<span data-ttu-id="f7992-386">**jämförelse**</span><span class="sxs-lookup"><span data-stu-id="f7992-386">**comparison**</span></span>|**=**<br /><br /> <span data-ttu-id="f7992-387">**!=, <>**</span><span class="sxs-lookup"><span data-stu-id="f7992-387">**!=, <>**</span></span><br /><br /> **>**<br /><br /> **>=**<br /><br /> **<**<br /><br /> **<=**<br /><br /> <span data-ttu-id="f7992-388">**??**</span><span class="sxs-lookup"><span data-stu-id="f7992-388">**??**</span></span>|<span data-ttu-id="f7992-389">Lika med.</span><span class="sxs-lookup"><span data-stu-id="f7992-389">Equals.</span></span> <span data-ttu-id="f7992-390">Returnerar **SANT** om argument är lika returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-390">Returns **true** if arguments are equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="f7992-391">Är inte lika med.</span><span class="sxs-lookup"><span data-stu-id="f7992-391">Not equal to.</span></span> <span data-ttu-id="f7992-392">Returnerar **SANT** om argumenten inte är lika, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-392">Returns **true** if arguments are not equal, returns **false** otherwise.</span></span><br /><br /> <span data-ttu-id="f7992-393">Större än.</span><span class="sxs-lookup"><span data-stu-id="f7992-393">Greater Than.</span></span> <span data-ttu-id="f7992-394">Returnerar **SANT** om det första argumentet är större än det andra returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-394">Returns **true** if first argument is greater than the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="f7992-395">Större än eller lika med.</span><span class="sxs-lookup"><span data-stu-id="f7992-395">Greater Than or Equal To.</span></span> <span data-ttu-id="f7992-396">Returnerar **SANT** om första argument är större än eller lika med det andra, returnera **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-396">Returns **true** if first argument is greater than or equal to the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="f7992-397">Mindre än.</span><span class="sxs-lookup"><span data-stu-id="f7992-397">Less Than.</span></span> <span data-ttu-id="f7992-398">Returnerar **SANT** om första argumentet är mindre än en sekund, returnera **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-398">Returns **true** if first argument is less than the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="f7992-399">Mindre än eller lika med.</span><span class="sxs-lookup"><span data-stu-id="f7992-399">Less Than or Equal To.</span></span> <span data-ttu-id="f7992-400">Returnerar **SANT** om det första argumentet är mindre än eller lika med det andra, returnerar **FALSKT** annars.</span><span class="sxs-lookup"><span data-stu-id="f7992-400">Returns **true** if first argument is less than or equal to the second one, return **false** otherwise.</span></span><br /><br /> <span data-ttu-id="f7992-401">Slå samman.</span><span class="sxs-lookup"><span data-stu-id="f7992-401">Coalesce.</span></span> <span data-ttu-id="f7992-402">Returnerar det andra argumentet om det första argumentet är en **Odefinierad** värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-402">Returns the second argument if the first argument is an **undefined** value.</span></span>|  
|<span data-ttu-id="f7992-403">**Sträng**</span><span class="sxs-lookup"><span data-stu-id="f7992-403">**String**</span></span>|<span data-ttu-id="f7992-404">**&#124;&#124;**</span><span class="sxs-lookup"><span data-stu-id="f7992-404">**&#124;&#124;**</span></span>|<span data-ttu-id="f7992-405">Sammanfogning.</span><span class="sxs-lookup"><span data-stu-id="f7992-405">Concatenation.</span></span> <span data-ttu-id="f7992-406">Returnerar en sammansättning av båda argumenten.</span><span class="sxs-lookup"><span data-stu-id="f7992-406">Returns a concatenation of both arguments.</span></span>|  
  
 <span data-ttu-id="f7992-407">**Ternär operator:**</span><span class="sxs-lookup"><span data-stu-id="f7992-407">**Ternary operators:**</span></span>  
  
|<span data-ttu-id="f7992-408">Ternär operator</span><span class="sxs-lookup"><span data-stu-id="f7992-408">Ternary operator</span></span>|<span data-ttu-id="f7992-409">?</span><span class="sxs-lookup"><span data-stu-id="f7992-409">?</span></span>|<span data-ttu-id="f7992-410">Returnerar det andra argumentet om det första argumentet evalueras till **SANT**; annars returnerar det tredje argumentet.</span><span class="sxs-lookup"><span data-stu-id="f7992-410">Returns the second argument if the first argument evaluates to **true**; return the third argument otherwise.</span></span>|  
|-|-|-|  
  
 <span data-ttu-id="f7992-411">**Sorteringen av värden för jämförelse**</span><span class="sxs-lookup"><span data-stu-id="f7992-411">**Ordering of values for comparison**</span></span>  
  
|<span data-ttu-id="f7992-412">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f7992-412">**Type**</span></span>|<span data-ttu-id="f7992-413">**Värden ordning**</span><span class="sxs-lookup"><span data-stu-id="f7992-413">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="f7992-414">**Odefinierad**</span><span class="sxs-lookup"><span data-stu-id="f7992-414">**Undefined**</span></span>|<span data-ttu-id="f7992-415">Inte jämförbar.</span><span class="sxs-lookup"><span data-stu-id="f7992-415">Not comparable.</span></span>|  
|<span data-ttu-id="f7992-416">**Null**</span><span class="sxs-lookup"><span data-stu-id="f7992-416">**Null**</span></span>|<span data-ttu-id="f7992-417">Enstaka värde: **null**</span><span class="sxs-lookup"><span data-stu-id="f7992-417">Single value: **null**</span></span>|  
|<span data-ttu-id="f7992-418">**Antal**</span><span class="sxs-lookup"><span data-stu-id="f7992-418">**Number**</span></span>|<span data-ttu-id="f7992-419">Naturliga tal.</span><span class="sxs-lookup"><span data-stu-id="f7992-419">Natural real number.</span></span><br /><br /> <span data-ttu-id="f7992-420">Negativt oändligt värde är mindre än numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-420">Negative Infinity value is smaller than any other Number value.</span></span><br /><br /> <span data-ttu-id="f7992-421">Positivt oändligt värde är större än värdet värde. **NaN** värde är inte jämförbar.</span><span class="sxs-lookup"><span data-stu-id="f7992-421">Positive Infinity value is larger than any other Number value.**NaN** value is not comparable.</span></span> <span data-ttu-id="f7992-422">Jämföra med **NaN** leder **Odefinierad** värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-422">Comparing with **NaN** will result in **undefined** value.</span></span>|  
|<span data-ttu-id="f7992-423">**Sträng**</span><span class="sxs-lookup"><span data-stu-id="f7992-423">**String**</span></span>|<span data-ttu-id="f7992-424">Lexicographical ordning.</span><span class="sxs-lookup"><span data-stu-id="f7992-424">Lexicographical order.</span></span>|  
|<span data-ttu-id="f7992-425">**Matris**</span><span class="sxs-lookup"><span data-stu-id="f7992-425">**Array**</span></span>|<span data-ttu-id="f7992-426">Ingen ordning, men balanserad.</span><span class="sxs-lookup"><span data-stu-id="f7992-426">No ordering, but equitable.</span></span>|  
|<span data-ttu-id="f7992-427">**Objektet**</span><span class="sxs-lookup"><span data-stu-id="f7992-427">**Object**</span></span>|<span data-ttu-id="f7992-428">Ingen ordning, men balanserad.</span><span class="sxs-lookup"><span data-stu-id="f7992-428">No ordering, but equitable.</span></span>|  
  
 <span data-ttu-id="f7992-429">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-429">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-430">I Azure Cosmos DB kallas ofta inte typerna av värden tills de faktiskt har hämtats från databasen.</span><span class="sxs-lookup"><span data-stu-id="f7992-430">In Azure Cosmos DB, the types of values are often not known until they are actually retrieved from the database.</span></span> <span data-ttu-id="f7992-431">För att stödja effektivt utförande av frågor har de flesta operatorer strikt krav.</span><span class="sxs-lookup"><span data-stu-id="f7992-431">In order to support efficient execution of queries, most of the operators have strict type requirements.</span></span> <span data-ttu-id="f7992-432">Operatörer ensamt utför även inte implicita konverteringar.</span><span class="sxs-lookup"><span data-stu-id="f7992-432">Also operators by themselves do not perform implicit conversions.</span></span>  
  
 <span data-ttu-id="f7992-433">Det innebär att en fråga som: Välj * från ROOT r var r.Age = 21 returnerar enbart dokument med egenskapen ålder lika med antalet 21.</span><span class="sxs-lookup"><span data-stu-id="f7992-433">This means that a query like: SELECT * FROM ROOT r WHERE r.Age = 21 will only return documents with property Age equal to the number 21.</span></span> <span data-ttu-id="f7992-434">Dokument med egenskapen ålder som är lika med strängen ”21” eller ”0021” strängen matchar inte, som uttryck ”21” = 21 utvärderar till odefinierad.</span><span class="sxs-lookup"><span data-stu-id="f7992-434">Documents with property Age equal to the string "21" or the string "0021" will not match, as the expression "21" = 21 evaluates to undefined.</span></span> <span data-ttu-id="f7992-435">Detta ger en bättre användning av index, eftersom sökning efter ett visst värde (d.v.s. number 21) är snabbare än att söka efter ett obestämt antal möjliga matchningar (dvs. antalet 21 eller strängar ”21”, ”021”, ”21.0”...).</span><span class="sxs-lookup"><span data-stu-id="f7992-435">This allows for a better use of indexes, because the lookup of a specific value (i.e. number 21) is faster than search for indefinite number of potential matches (i.e. number 21 or strings "21", "021", "21.0" …).</span></span> <span data-ttu-id="f7992-436">Detta skiljer sig från hur JavaScript utvärderar operatorer på värden av olika typer.</span><span class="sxs-lookup"><span data-stu-id="f7992-436">This is different from how JavaScript evaluates operators on values of different types.</span></span>  
  
 <span data-ttu-id="f7992-437">**Jämförelse av och likhetsfrågor för matriser och -objekt**</span><span class="sxs-lookup"><span data-stu-id="f7992-437">**Arrays and objects equality and comparison**</span></span>  
  
 <span data-ttu-id="f7992-438">Jämförelse av värden för matris eller ett objekt som använder range-operatorer (>, > =, <, < =) leder till odefinierad eftersom det inte finns inte ordning som har definierats för objektet eller matrisen värden.</span><span class="sxs-lookup"><span data-stu-id="f7992-438">Comparing of Array or Object values using range operators (>, >=, <, <=) will result in undefined as there is not order defined on Object or Array values.</span></span> <span data-ttu-id="f7992-439">Men använder likhet/olikhet operatorer (=,! = <>) stöds och värden som ska jämföras strukturellt.</span><span class="sxs-lookup"><span data-stu-id="f7992-439">However using equality/inequality operators (=, !=, <>) is supported and values will be compared structurally.</span></span>  
  
 <span data-ttu-id="f7992-440">Matriser är lika om båda matriser har samma antal element och elementen på matchar positioner också är lika.</span><span class="sxs-lookup"><span data-stu-id="f7992-440">Arrays are equal if both arrays have same number of elements and elements at matching positions are also equal.</span></span> <span data-ttu-id="f7992-441">Om att jämföra element resulterar i ett par Odefinierad är resultatet av matrisen jämförelse odefinierad.</span><span class="sxs-lookup"><span data-stu-id="f7992-441">If comparing any pair of elements results in undefined, the result of array comparison is undefined.</span></span>  
  
 <span data-ttu-id="f7992-442">Objekt är lika om både objekt har samma egenskaper som definierats och värden av matchande egenskaper också är lika.</span><span class="sxs-lookup"><span data-stu-id="f7992-442">Objects are equal if both objects have same properties defined, and if values of matching properties are also equal.</span></span> <span data-ttu-id="f7992-443">Om att jämföra ett par värden resultat Odefinierad är resultatet av objektet jämförelse odefinierad.</span><span class="sxs-lookup"><span data-stu-id="f7992-443">If comparing any pair of property values results in undefined, the result of object comparison is undefined.</span></span>  
  
##  <span data-ttu-id="f7992-444"><a name="bk_constants"></a>Konstanter</span><span class="sxs-lookup"><span data-stu-id="f7992-444"><a name="bk_constants"></a> Constants</span></span>  
 <span data-ttu-id="f7992-445">En konstant, även kallat ett litteralvärde eller ett skalärt värde är en symbol som representerar ett specifikt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-445">A constant, also known as a literal or a scalar value, is a symbol that represents a specific data value.</span></span> <span data-ttu-id="f7992-446">Formatet för en konstant beror på det värde som representerar datatyp.</span><span class="sxs-lookup"><span data-stu-id="f7992-446">The format of a constant depends on the data type of the value it represents.</span></span>  
  
 <span data-ttu-id="f7992-447">**Skalära datatyper som stöds:**</span><span class="sxs-lookup"><span data-stu-id="f7992-447">**Supported scalar data types:**</span></span>  
  
|<span data-ttu-id="f7992-448">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f7992-448">**Type**</span></span>|<span data-ttu-id="f7992-449">**Värden ordning**</span><span class="sxs-lookup"><span data-stu-id="f7992-449">**Values order**</span></span>|  
|-|-|  
|<span data-ttu-id="f7992-450">**Odefinierad**</span><span class="sxs-lookup"><span data-stu-id="f7992-450">**Undefined**</span></span>|<span data-ttu-id="f7992-451">Enstaka värde: **Odefinierad**</span><span class="sxs-lookup"><span data-stu-id="f7992-451">Single value: **undefined**</span></span>|  
|<span data-ttu-id="f7992-452">**Null**</span><span class="sxs-lookup"><span data-stu-id="f7992-452">**Null**</span></span>|<span data-ttu-id="f7992-453">Enstaka värde: **null**</span><span class="sxs-lookup"><span data-stu-id="f7992-453">Single value: **null**</span></span>|  
|<span data-ttu-id="f7992-454">**Booleskt värde**</span><span class="sxs-lookup"><span data-stu-id="f7992-454">**Boolean**</span></span>|<span data-ttu-id="f7992-455">Värden: **FALSKT**, **SANT**.</span><span class="sxs-lookup"><span data-stu-id="f7992-455">Values: **false**, **true**.</span></span>|  
|<span data-ttu-id="f7992-456">**Antal**</span><span class="sxs-lookup"><span data-stu-id="f7992-456">**Number**</span></span>|<span data-ttu-id="f7992-457">En dubbel precision flyttal, IEEE-754 som standard.</span><span class="sxs-lookup"><span data-stu-id="f7992-457">A double-precision floating-point number, IEEE 754 standard.</span></span>|  
|<span data-ttu-id="f7992-458">**Sträng**</span><span class="sxs-lookup"><span data-stu-id="f7992-458">**String**</span></span>|<span data-ttu-id="f7992-459">En sekvens med noll eller flera Unicode-tecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-459">A sequence of zero or more Unicode characters.</span></span> <span data-ttu-id="f7992-460">Strängar måste stå inom enkla eller dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-460">Strings must be enclosed in single or double quotes.</span></span>|  
|<span data-ttu-id="f7992-461">**Matris**</span><span class="sxs-lookup"><span data-stu-id="f7992-461">**Array**</span></span>|<span data-ttu-id="f7992-462">En sekvens med noll eller flera element.</span><span class="sxs-lookup"><span data-stu-id="f7992-462">A sequence of zero or more elements.</span></span> <span data-ttu-id="f7992-463">Varje element kan vara ett värde med en skalär datatypen, utom Undefined.</span><span class="sxs-lookup"><span data-stu-id="f7992-463">Each element can be a value of any scalar data type, except Undefined.</span></span>|  
|<span data-ttu-id="f7992-464">**Objektet**</span><span class="sxs-lookup"><span data-stu-id="f7992-464">**Object**</span></span>|<span data-ttu-id="f7992-465">En osorterad uppsättning noll eller flera namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="f7992-465">An unordered set of zero or more name/value pairs.</span></span> <span data-ttu-id="f7992-466">Namnet är en Unicode-sträng, värdet kan vara av valfri skalära datatyp, utom **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="f7992-466">Name is a Unicode string, value can be of any scalar data type, except **Undefined**.</span></span>|  
  
 <span data-ttu-id="f7992-467">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-467">**Syntax**</span></span>  
  
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
  
 <span data-ttu-id="f7992-468">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-468">**Arguments**</span></span>  
  
1.  `<undefined_constant>; undefined`  
  
     <span data-ttu-id="f7992-469">Representerar odefinierat värde av typen odefinierad.</span><span class="sxs-lookup"><span data-stu-id="f7992-469">Represents undefined value of type Undefined.</span></span>  
  
2.  `<null_constant>; null`  
  
     <span data-ttu-id="f7992-470">Representerar **null** värde av typen **Null**.</span><span class="sxs-lookup"><span data-stu-id="f7992-470">Represents **null** value of type **Null**.</span></span>  
  
3.  `<boolean_constant>`  
  
     <span data-ttu-id="f7992-471">Representerar konstanter av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="f7992-471">Represents constant of type Boolean.</span></span>  
  
4.  `false`  
  
     <span data-ttu-id="f7992-472">Representerar **FALSKT** värde av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="f7992-472">Represents **false** value of type Boolean.</span></span>  
  
5.  `true`  
  
     <span data-ttu-id="f7992-473">Representerar **SANT** värde av typen Boolean.</span><span class="sxs-lookup"><span data-stu-id="f7992-473">Represents **true** value of type Boolean.</span></span>  
  
6.  `<number_constant>`  
  
     <span data-ttu-id="f7992-474">Representerar en konstant.</span><span class="sxs-lookup"><span data-stu-id="f7992-474">Represents a constant.</span></span>  
  
7.  `decimal_literal`  
  
     <span data-ttu-id="f7992-475">Decimal literaler är nummer som representeras med decimalform eller matematisk notation.</span><span class="sxs-lookup"><span data-stu-id="f7992-475">Decimal literals are numbers represented using either decimal notation, or scientific notation.</span></span>  
  
8.  `hexadecimal_literal`  
  
     <span data-ttu-id="f7992-476">Hexadecimala strängar är nummer som representeras med prefixet '0 x' följt av en eller flera hexadecimala siffror.</span><span class="sxs-lookup"><span data-stu-id="f7992-476">Hexadecimal literals are numbers represented using prefix '0x' followed by one or more hexadecimal digits.</span></span>  
  
9. `<string_constant>`  
  
     <span data-ttu-id="f7992-477">Representerar en konstant av typen String.</span><span class="sxs-lookup"><span data-stu-id="f7992-477">Represents a constant of type String.</span></span>  
  
10. `string _literal`  
  
     <span data-ttu-id="f7992-478">Stränglitteraler är Unicode-strängar som representeras av en följd av noll eller flera Unicode-tecken eller escape-sekvenser.</span><span class="sxs-lookup"><span data-stu-id="f7992-478">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="f7992-479">Stränglitteraler omges av enkla citattecken (apostrof: ') eller dubbla citattecken (citattecken ”:).</span><span class="sxs-lookup"><span data-stu-id="f7992-479">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span>  
  
 <span data-ttu-id="f7992-480">Följande escape-sekvenser tillåts:</span><span class="sxs-lookup"><span data-stu-id="f7992-480">Following escape sequences are allowed:</span></span>  
  
|<span data-ttu-id="f7992-481">**Escape-sekvens**</span><span class="sxs-lookup"><span data-stu-id="f7992-481">**Escape sequence**</span></span>|<span data-ttu-id="f7992-482">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="f7992-482">**Description**</span></span>|<span data-ttu-id="f7992-483">**Unicode-tecken**</span><span class="sxs-lookup"><span data-stu-id="f7992-483">**Unicode character**</span></span>|  
|-|-|-|  
|<span data-ttu-id="f7992-484">\\'</span><span class="sxs-lookup"><span data-stu-id="f7992-484">\\'</span></span>|<span data-ttu-id="f7992-485">apostrof (')</span><span class="sxs-lookup"><span data-stu-id="f7992-485">apostrophe (')</span></span>|<span data-ttu-id="f7992-486">U + 0027</span><span class="sxs-lookup"><span data-stu-id="f7992-486">U+0027</span></span>|  
|<span data-ttu-id="f7992-487">\\"</span><span class="sxs-lookup"><span data-stu-id="f7992-487">\\"</span></span>|<span data-ttu-id="f7992-488">citattecken (”)</span><span class="sxs-lookup"><span data-stu-id="f7992-488">quotation mark (")</span></span>|<span data-ttu-id="f7992-489">U + 0022</span><span class="sxs-lookup"><span data-stu-id="f7992-489">U+0022</span></span>|  
|\\\|<span data-ttu-id="f7992-490">omvänd solidus (\\)</span><span class="sxs-lookup"><span data-stu-id="f7992-490">reverse solidus (\\)</span></span>|<span data-ttu-id="f7992-491">U + 005C</span><span class="sxs-lookup"><span data-stu-id="f7992-491">U+005C</span></span>|  
|\\/|<span data-ttu-id="f7992-492">solidus (/)</span><span class="sxs-lookup"><span data-stu-id="f7992-492">solidus (/)</span></span>|<span data-ttu-id="f7992-493">U + 002F</span><span class="sxs-lookup"><span data-stu-id="f7992-493">U+002F</span></span>|  
|<span data-ttu-id="f7992-494">\b</span><span class="sxs-lookup"><span data-stu-id="f7992-494">\b</span></span>|<span data-ttu-id="f7992-495">BACKSTEG</span><span class="sxs-lookup"><span data-stu-id="f7992-495">backspace</span></span>|<span data-ttu-id="f7992-496">U + 0008</span><span class="sxs-lookup"><span data-stu-id="f7992-496">U+0008</span></span>|  
|<span data-ttu-id="f7992-497">\f</span><span class="sxs-lookup"><span data-stu-id="f7992-497">\f</span></span>|<span data-ttu-id="f7992-498">formuläret feed</span><span class="sxs-lookup"><span data-stu-id="f7992-498">form feed</span></span>|<span data-ttu-id="f7992-499">U + 000C</span><span class="sxs-lookup"><span data-stu-id="f7992-499">U+000C</span></span>|  
|\n|<span data-ttu-id="f7992-500">radmatning</span><span class="sxs-lookup"><span data-stu-id="f7992-500">line feed</span></span>|<span data-ttu-id="f7992-501">U + 000A</span><span class="sxs-lookup"><span data-stu-id="f7992-501">U+000A</span></span>|  
|<span data-ttu-id="f7992-502">\r</span><span class="sxs-lookup"><span data-stu-id="f7992-502">\r</span></span>|<span data-ttu-id="f7992-503">vagnretur</span><span class="sxs-lookup"><span data-stu-id="f7992-503">carriage return</span></span>|<span data-ttu-id="f7992-504">U + 000D</span><span class="sxs-lookup"><span data-stu-id="f7992-504">U+000D</span></span>|  
|<span data-ttu-id="f7992-505">\t</span><span class="sxs-lookup"><span data-stu-id="f7992-505">\t</span></span>|<span data-ttu-id="f7992-506">fliken</span><span class="sxs-lookup"><span data-stu-id="f7992-506">tab</span></span>|<span data-ttu-id="f7992-507">U + 0009</span><span class="sxs-lookup"><span data-stu-id="f7992-507">U+0009</span></span>|  
|<span data-ttu-id="f7992-508">\uXXXX</span><span class="sxs-lookup"><span data-stu-id="f7992-508">\uXXXX</span></span>|<span data-ttu-id="f7992-509">En Unicode-tecken som definieras av 4 hexadecimala siffror.</span><span class="sxs-lookup"><span data-stu-id="f7992-509">A Unicode character defined by 4 hexadecimal digits.</span></span>|<span data-ttu-id="f7992-510">U + XXXX</span><span class="sxs-lookup"><span data-stu-id="f7992-510">U+XXXX</span></span>|  
  
##  <span data-ttu-id="f7992-511"><a name="bk_query_perf_guidelines"></a>Riktlinjer för frågan prestanda</span><span class="sxs-lookup"><span data-stu-id="f7992-511"><a name="bk_query_perf_guidelines"></a> Query performance guidelines</span></span>  
 <span data-ttu-id="f7992-512">Det bör använda filter som kan hanteras via ett eller flera index för en fråga som ska köras effektivt för en stor samling.</span><span class="sxs-lookup"><span data-stu-id="f7992-512">In order for a query to be executed efficiently for a large collection, it should use filters which can be served through one or more indexes.</span></span>  
  
 <span data-ttu-id="f7992-513">Följande filter beaktas för index sökning:</span><span class="sxs-lookup"><span data-stu-id="f7992-513">The following filters will be considered for index lookup:</span></span>  
  
-   <span data-ttu-id="f7992-514">Använd Likhetsoperatorn (=) med ett sökvägsuttryck för dokumentet och en konstant.</span><span class="sxs-lookup"><span data-stu-id="f7992-514">Use equality operator ( = ) with a document path expression and a constant.</span></span>  
  
-   <span data-ttu-id="f7992-515">Använd range-operatorer (<, \<=, >, > =) med ett sökvägsuttryck för dokumentet och antalet konstanter.</span><span class="sxs-lookup"><span data-stu-id="f7992-515">Use range operators (<, \<=, >, >=) with a document path expression and number constants.</span></span>  
  
-   <span data-ttu-id="f7992-516">Dokumentet sökvägsuttryck står för ett uttryck som identifierar en konstant sökväg i dokument från samlingen refererad databas.</span><span class="sxs-lookup"><span data-stu-id="f7992-516">Document path expression stands for any expression which identifies a constant path in the documents from the referenced database collection.</span></span>  
  
 <span data-ttu-id="f7992-517">**Dokumentet sökvägsuttryck**</span><span class="sxs-lookup"><span data-stu-id="f7992-517">**Document path expression**</span></span>  
  
 <span data-ttu-id="f7992-518">Dokumentet sökvägsuttryck är uttryck som en sökväg till matrisen egenskapen eller indexeraren bedömare över ett dokument som kommer från databasen samling dokument.</span><span class="sxs-lookup"><span data-stu-id="f7992-518">Document path expressions are expressions that a path of property or array indexer assessors over a document coming from database collection documents.</span></span> <span data-ttu-id="f7992-519">Den här sökvägen kan användas för att identifiera platsen för värden som refereras i ett filter direkt i dokumenten i samlingen databasen.</span><span class="sxs-lookup"><span data-stu-id="f7992-519">This path can be used to identify the location of values referenced in a filter directly within the documents in the database collection.</span></span>  
  
 <span data-ttu-id="f7992-520">För ett uttryck för att ses som ett dokument sökvägsuttryck bör det:</span><span class="sxs-lookup"><span data-stu-id="f7992-520">For an expression to be considered a document path expression, it should:</span></span>  
  
1.  <span data-ttu-id="f7992-521">Referera till samlingen roten direkt.</span><span class="sxs-lookup"><span data-stu-id="f7992-521">Reference the collection root directly.</span></span>  
  
2.  <span data-ttu-id="f7992-522">Indexerare för referens-egenskapen eller konstant matris för vissa dokument sökvägsuttryck</span><span class="sxs-lookup"><span data-stu-id="f7992-522">Reference property or constant array indexer of some document path expression</span></span>  
  
3.  <span data-ttu-id="f7992-523">Referera till ett alias som representerar vissa dokument sökvägsuttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-523">Reference an alias, which represents some document path expression.</span></span>  
  
     <span data-ttu-id="f7992-524">**Syntaxen konventioner**</span><span class="sxs-lookup"><span data-stu-id="f7992-524">**Syntax conventions**</span></span>  
  
     <span data-ttu-id="f7992-525">I följande tabell beskrivs konventioner används för att beskriva syntax i frågespråket i DocumentDB API-referens.</span><span class="sxs-lookup"><span data-stu-id="f7992-525">The following table describes the conventions used to describe syntax in the DocumentDB API Query Language reference.</span></span>  
  
    |<span data-ttu-id="f7992-526">**Konventionen**</span><span class="sxs-lookup"><span data-stu-id="f7992-526">**Convention**</span></span>|<span data-ttu-id="f7992-527">**Används för**</span><span class="sxs-lookup"><span data-stu-id="f7992-527">**Used for**</span></span>|  
    |-|-|    
    |<span data-ttu-id="f7992-528">VERSALER</span><span class="sxs-lookup"><span data-stu-id="f7992-528">UPPERCASE</span></span>|<span data-ttu-id="f7992-529">Skiftlägeskänsliga nyckelord.</span><span class="sxs-lookup"><span data-stu-id="f7992-529">Case-insensitive keywords.</span></span>|  
    |<span data-ttu-id="f7992-530">gemener</span><span class="sxs-lookup"><span data-stu-id="f7992-530">lowercase</span></span>|<span data-ttu-id="f7992-531">Skiftlägeskänsligt nyckelord.</span><span class="sxs-lookup"><span data-stu-id="f7992-531">Case-sensitive keywords.</span></span>|  
    |<span data-ttu-id="f7992-532">\<nonterminal ></span><span class="sxs-lookup"><span data-stu-id="f7992-532">\<nonterminal></span></span>|<span data-ttu-id="f7992-533">Öppen, som har definierats separat.</span><span class="sxs-lookup"><span data-stu-id="f7992-533">Nonterminal, defined separately.</span></span>|  
    |<span data-ttu-id="f7992-534">\<nonterminal >:: =</span><span class="sxs-lookup"><span data-stu-id="f7992-534">\<nonterminal> ::=</span></span>|<span data-ttu-id="f7992-535">Syntax för definition av nonterminal.</span><span class="sxs-lookup"><span data-stu-id="f7992-535">Syntax definition of the nonterminal.</span></span>|  
    |<span data-ttu-id="f7992-536">other_terminal</span><span class="sxs-lookup"><span data-stu-id="f7992-536">other_terminal</span></span>|<span data-ttu-id="f7992-537">Terminal (token), som beskrivs i detalj i ord.</span><span class="sxs-lookup"><span data-stu-id="f7992-537">Terminal (token), described in detail in words.</span></span>|  
    |<span data-ttu-id="f7992-538">Identifierare</span><span class="sxs-lookup"><span data-stu-id="f7992-538">identifier</span></span>|<span data-ttu-id="f7992-539">Identifierare.</span><span class="sxs-lookup"><span data-stu-id="f7992-539">Identifier.</span></span> <span data-ttu-id="f7992-540">Gör följande endast tecken: a-z A-Z 0-9 _First tecknet får inte vara en siffra.</span><span class="sxs-lookup"><span data-stu-id="f7992-540">Allows following characters only: a-z A-Z 0-9 _First character cannot be a digit.</span></span>|  
    |<span data-ttu-id="f7992-541">”sträng”</span><span class="sxs-lookup"><span data-stu-id="f7992-541">"string"</span></span>|<span data-ttu-id="f7992-542">Sträng inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-542">Quoted string.</span></span> <span data-ttu-id="f7992-543">Gör att en giltig sträng.</span><span class="sxs-lookup"><span data-stu-id="f7992-543">Allows any valid string.</span></span> <span data-ttu-id="f7992-544">Se beskrivning av en string_literal.</span><span class="sxs-lookup"><span data-stu-id="f7992-544">See description of a string_literal.</span></span>|  
    |<span data-ttu-id="f7992-545">'symbolen'</span><span class="sxs-lookup"><span data-stu-id="f7992-545">'symbol'</span></span>|<span data-ttu-id="f7992-546">Literalen symbolen som är del av syntaxen.</span><span class="sxs-lookup"><span data-stu-id="f7992-546">Literal symbol which is part of the syntax.</span></span>|  
    |<span data-ttu-id="f7992-547">&#124; (lodrätt streck)</span><span class="sxs-lookup"><span data-stu-id="f7992-547">&#124; (vertical bar)</span></span>|<span data-ttu-id="f7992-548">Alternativ för syntax objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-548">Alternatives for syntax items.</span></span> <span data-ttu-id="f7992-549">Du kan använda endast en av de angivna objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-549">You can use only one of the items specified.</span></span>|  
    |<span data-ttu-id="f7992-550">[] /(brackets)</span><span class="sxs-lookup"><span data-stu-id="f7992-550">[ ] /(brackets)</span></span>|<span data-ttu-id="f7992-551">Hakparenteser innefatta en eller flera valfria objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-551">Brackets enclose one or more optional items.</span></span>|  
    |<span data-ttu-id="f7992-552">[,.. .n]</span><span class="sxs-lookup"><span data-stu-id="f7992-552">[ ,...n ]</span></span>|<span data-ttu-id="f7992-553">Anger det föregående element kan vara upprepade n är antalet gånger.</span><span class="sxs-lookup"><span data-stu-id="f7992-553">Indicates the preceding item can be repeated n number of times.</span></span> <span data-ttu-id="f7992-554">Förekomster avgränsas med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-554">The occurrences are separated by commas.</span></span>|  
    |<span data-ttu-id="f7992-555">[.. .n]</span><span class="sxs-lookup"><span data-stu-id="f7992-555">[ ...n ]</span></span>|<span data-ttu-id="f7992-556">Anger det föregående element kan vara upprepade n är antalet gånger.</span><span class="sxs-lookup"><span data-stu-id="f7992-556">Indicates the preceding item can be repeated n number of times.</span></span> <span data-ttu-id="f7992-557">Förekomster avgränsas med blanksteg.</span><span class="sxs-lookup"><span data-stu-id="f7992-557">The occurrences are separated by blanks.</span></span>|  
  
##  <span data-ttu-id="f7992-558"><a name="bk_built_in_functions"></a>Inbyggda funktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-558"><a name="bk_built_in_functions"></a> Built-in functions</span></span>  
 <span data-ttu-id="f7992-559">Azure Cosmos-DB innehåller många inbyggda SQL-funktioner.</span><span class="sxs-lookup"><span data-stu-id="f7992-559">Azure Cosmos DB provides many built-in SQL functions.</span></span> <span data-ttu-id="f7992-560">Kategorier av inbyggda funktioner i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="f7992-560">The categories of built-in functions are listed below.</span></span>  
  
|<span data-ttu-id="f7992-561">Funktionen</span><span class="sxs-lookup"><span data-stu-id="f7992-561">Function</span></span>|<span data-ttu-id="f7992-562">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f7992-562">Description</span></span>|  
|--------------|-----------------|  
|[<span data-ttu-id="f7992-563">Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-563">Mathematical functions</span></span>](#bk_mathematical_functions)|<span data-ttu-id="f7992-564">Matematiska funktioner utför en beräkning, vanligtvis baserat på värden som har angetts som argument och returnerar ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-564">The mathematical functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>|  
|[<span data-ttu-id="f7992-565">Ange kontrollerar funktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-565">Type checking functions</span></span>](#bk_type_checking_functions)|<span data-ttu-id="f7992-566">Typen kontrollerar funktioner kan du kontrollera vilken typ av ett uttryck i SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="f7992-566">The type checking functions allow you to check the type of an expression within SQL queries.</span></span>|  
|[<span data-ttu-id="f7992-567">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-567">String functions</span></span>](#bk_string_functions)|<span data-ttu-id="f7992-568">Strängfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-568">The string functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>|  
|[<span data-ttu-id="f7992-569">Matrisfunktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-569">Array functions</span></span>](#bk_array_functions)|<span data-ttu-id="f7992-570">Matrisfunktioner kan du utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-570">The array functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span>|  
|[<span data-ttu-id="f7992-571">Spatial funktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-571">Spatial functions</span></span>](#bk_spatial_functions)|<span data-ttu-id="f7992-572">Spatial funktioner utföra en åtgärd på ett indatavärde Rumsobjektet och returnera ett numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-572">The spatial functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>|  
  
###  <span data-ttu-id="f7992-573"><a name="bk_mathematical_functions"></a>Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-573"><a name="bk_mathematical_functions"></a> Mathematical functions</span></span>  
 <span data-ttu-id="f7992-574">Följande funktioner utför en beräkning, vanligtvis baserat på värden som har angetts som argument och returnerar ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-574">The following functions each perform a calculation, usually based on input values that are provided as arguments, and return a numeric value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="f7992-575">ABS</span><span class="sxs-lookup"><span data-stu-id="f7992-575">ABS</span></span>](#bk_abs)|[<span data-ttu-id="f7992-576">ARCCOS</span><span class="sxs-lookup"><span data-stu-id="f7992-576">ACOS</span></span>](#bk_acos)|[<span data-ttu-id="f7992-577">ARCSIN</span><span class="sxs-lookup"><span data-stu-id="f7992-577">ASIN</span></span>](#bk_asin)|  
|[<span data-ttu-id="f7992-578">ARCTAN</span><span class="sxs-lookup"><span data-stu-id="f7992-578">ATAN</span></span>](#bk_atan)|[<span data-ttu-id="f7992-579">ATN2</span><span class="sxs-lookup"><span data-stu-id="f7992-579">ATN2</span></span>](#bk_atn2)|[<span data-ttu-id="f7992-580">TAK</span><span class="sxs-lookup"><span data-stu-id="f7992-580">CEILING</span></span>](#bk_ceiling)|  
|[<span data-ttu-id="f7992-581">COS</span><span class="sxs-lookup"><span data-stu-id="f7992-581">COS</span></span>](#bk_cos)|[<span data-ttu-id="f7992-582">BET</span><span class="sxs-lookup"><span data-stu-id="f7992-582">COT</span></span>](#bk_cot)|[<span data-ttu-id="f7992-583">GRADER</span><span class="sxs-lookup"><span data-stu-id="f7992-583">DEGREES</span></span>](#bk_degrees)|  
|[<span data-ttu-id="f7992-584">EXP</span><span class="sxs-lookup"><span data-stu-id="f7992-584">EXP</span></span>](#bk_exp)|[<span data-ttu-id="f7992-585">VÅNING</span><span class="sxs-lookup"><span data-stu-id="f7992-585">FLOOR</span></span>](#bk_floor)|[<span data-ttu-id="f7992-586">LOGG</span><span class="sxs-lookup"><span data-stu-id="f7992-586">LOG</span></span>](#bk_log)|  
|[<span data-ttu-id="f7992-587">LOG10</span><span class="sxs-lookup"><span data-stu-id="f7992-587">LOG10</span></span>](#bk_log10)|[<span data-ttu-id="f7992-588">PI</span><span class="sxs-lookup"><span data-stu-id="f7992-588">PI</span></span>](#bk_pi)|[<span data-ttu-id="f7992-589">POWER</span><span class="sxs-lookup"><span data-stu-id="f7992-589">POWER</span></span>](#bk_power)|  
|[<span data-ttu-id="f7992-590">RADIANER</span><span class="sxs-lookup"><span data-stu-id="f7992-590">RADIANS</span></span>](#bk_radians)|[<span data-ttu-id="f7992-591">AVRUNDA</span><span class="sxs-lookup"><span data-stu-id="f7992-591">ROUND</span></span>](#bk_round)|[<span data-ttu-id="f7992-592">SIN</span><span class="sxs-lookup"><span data-stu-id="f7992-592">SIN</span></span>](#bk_sin)|  
|[<span data-ttu-id="f7992-593">ROT</span><span class="sxs-lookup"><span data-stu-id="f7992-593">SQRT</span></span>](#bk_sqrt)|[<span data-ttu-id="f7992-594">RUTA</span><span class="sxs-lookup"><span data-stu-id="f7992-594">SQUARE</span></span>](#bk_square)|[<span data-ttu-id="f7992-595">LOGGA</span><span class="sxs-lookup"><span data-stu-id="f7992-595">SIGN</span></span>](#bk_sign)|  
|[<span data-ttu-id="f7992-596">TAN</span><span class="sxs-lookup"><span data-stu-id="f7992-596">TAN</span></span>](#bk_tan)|[<span data-ttu-id="f7992-597">AVKORTA</span><span class="sxs-lookup"><span data-stu-id="f7992-597">TRUNC</span></span>](#bk_trunc)||  
  
####  <span data-ttu-id="f7992-598"><a name="bk_abs"></a>ABS</span><span class="sxs-lookup"><span data-stu-id="f7992-598"><a name="bk_abs"></a> ABS</span></span>  
 <span data-ttu-id="f7992-599">Returnerar det absoluta (positiva) värdet av uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-599">Returns the absolute (positive) value of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-600">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-600">**Syntax**</span></span>  
  
```  
ABS (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-601">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-601">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-602">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-602">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-603">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-603">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-604">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-604">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-605">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-605">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-606">I följande exempel visas resultatet av att använda funktionen ABS på tre olika nummer.</span><span class="sxs-lookup"><span data-stu-id="f7992-606">The following example shows the results of using the ABS function on three different numbers.</span></span>  
  
```  
SELECT ABS(-1), ABS(0), ABS(1)  
```  
  
 <span data-ttu-id="f7992-607">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-607">Here is the result set.</span></span>  
  
```  
[{$1: 1, $2: 0, $3: 1}]  
```  
  
####  <span data-ttu-id="f7992-608"><a name="bk_acos"></a>ARCCOS</span><span class="sxs-lookup"><span data-stu-id="f7992-608"><a name="bk_acos"></a> ACOS</span></span>  
 <span data-ttu-id="f7992-609">Returnerar vinkeln i radianer, vars cosinus är det angivna numeriska uttrycket; kallas även cosinus.</span><span class="sxs-lookup"><span data-stu-id="f7992-609">Returns the angle, in radians, whose cosine is the specified numeric expression; also called arccosine.</span></span>  
  
 <span data-ttu-id="f7992-610">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-610">**Syntax**</span></span>  
  
```  
ACOS(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-611">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-611">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-612">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-612">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-613">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-613">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-614">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-614">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-615">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-615">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-616">I följande exempel returnerar ARCCOS 1.</span><span class="sxs-lookup"><span data-stu-id="f7992-616">The following example returns the ACOS of -1.</span></span>  
  
```  
SELECT ACOS(-1)  
```  
  
 <span data-ttu-id="f7992-617">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-617">Here is the result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="f7992-618"><a name="bk_asin"></a>ARCSIN</span><span class="sxs-lookup"><span data-stu-id="f7992-618"><a name="bk_asin"></a> ASIN</span></span>  
 <span data-ttu-id="f7992-619">Returnerar vinkeln i radianer, vars sinus är ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-619">Returns the angle, in radians, whose sine is the specified numeric expression.</span></span> <span data-ttu-id="f7992-620">Detta kallas också arcsinus.</span><span class="sxs-lookup"><span data-stu-id="f7992-620">This is also called arcsine.</span></span>  
  
 <span data-ttu-id="f7992-621">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-621">**Syntax**</span></span>  
  
```  
ASIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-622">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-622">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-623">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-623">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-624">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-624">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-625">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-625">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-626">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-626">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-627">I följande exempel returnerar ARCSIN 1.</span><span class="sxs-lookup"><span data-stu-id="f7992-627">The following example returns the ASIN of -1.</span></span>  
  
```  
SELECT ASIN(-1)  
```  
  
 <span data-ttu-id="f7992-628">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-628">Here is the result set.</span></span>  
  
```  
[{"$1": -1.5707963267948966}]  
```  
  
####  <span data-ttu-id="f7992-629"><a name="bk_atan"></a>ARCTAN</span><span class="sxs-lookup"><span data-stu-id="f7992-629"><a name="bk_atan"></a> ATAN</span></span>  
 <span data-ttu-id="f7992-630">Returnerar vinkeln i radianer, vars tangens är ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-630">Returns the angle, in radians, whose tangent is the specified numeric expression.</span></span> <span data-ttu-id="f7992-631">Detta kallas också tangens.</span><span class="sxs-lookup"><span data-stu-id="f7992-631">This is also called arctangent.</span></span>  
  
 <span data-ttu-id="f7992-632">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-632">**Syntax**</span></span>  
  
```  
ATAN(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-633">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-633">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-634">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-634">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-635">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-635">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-636">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-636">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-637">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-637">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-638">I följande exempel returnerar ARCTAN för det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="f7992-638">The following example returns the ATAN of the specified value.</span></span>  
  
```  
SELECT ATAN(-45.01)  
```  
  
 <span data-ttu-id="f7992-639">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-639">Here is the result set.</span></span>  
  
```  
[{"$1": -1.5485826962062663}]  
```  
  
####  <span data-ttu-id="f7992-640"><a name="bk_atn2"></a>ATN2</span><span class="sxs-lookup"><span data-stu-id="f7992-640"><a name="bk_atn2"></a> ATN2</span></span>  
 <span data-ttu-id="f7992-641">Returnerar huvudvärdet arctangens av y / x, uttryckt i radianer.</span><span class="sxs-lookup"><span data-stu-id="f7992-641">Returns the principal value of the arc tangent of y/x, expressed in radians.</span></span>  
  
 <span data-ttu-id="f7992-642">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-642">**Syntax**</span></span>  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-643">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-643">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-644">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-644">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-645">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-645">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-646">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-646">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-647">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-647">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-648">I följande exempel beräknas ATN2 för den angivna x och y komponenter.</span><span class="sxs-lookup"><span data-stu-id="f7992-648">The following example calculates the ATN2 for the specified x and y components.</span></span>  
  
```  
SELECT ATN2(35.175643, 129.44)  
```  
  
 <span data-ttu-id="f7992-649">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-649">Here is the result set.</span></span>  
  
```  
[{"$1": 1.3054517947300646}]  
```  
  
####  <span data-ttu-id="f7992-650"><a name="bk_ceiling"></a>TAK</span><span class="sxs-lookup"><span data-stu-id="f7992-650"><a name="bk_ceiling"></a> CEILING</span></span>  
 <span data-ttu-id="f7992-651">Returnerar det minsta heltalsvärdet större än eller lika med det angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-651">Returns the smallest integer value greater than, or equal to, the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-652">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-652">**Syntax**</span></span>  
  
```  
CEILING (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-653">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-653">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-654">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-654">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-655">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-655">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-656">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-656">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-657">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-657">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-658">I följande exempel visas positivt numeriskt och negativa noll värden med funktionen tak.</span><span class="sxs-lookup"><span data-stu-id="f7992-658">The following example shows positive numeric, negative, and zero values with the CEILING function.</span></span>  
  
```  
SELECT CEILING(123.45), CEILING(-123.45), CEILING(0.0)  
```  
  
 <span data-ttu-id="f7992-659">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-659">Here is the result set.</span></span>  
  
```  
[{$1: 124, $2: -123, $3: 0}]  
```  
  
####  <span data-ttu-id="f7992-660"><a name="bk_cos"></a>COS</span><span class="sxs-lookup"><span data-stu-id="f7992-660"><a name="bk_cos"></a> COS</span></span>  
 <span data-ttu-id="f7992-661">Returnerar trigonometriska värden cosinus för den angivna vinkeln i radianer i det angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-661">Returns the trigonometric cosine of the specified angle, in radians, in the specified expression.</span></span>  
  
 <span data-ttu-id="f7992-662">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-662">**Syntax**</span></span>  
  
```  
COS(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-663">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-663">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-664">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-664">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-665">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-665">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-666">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-666">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-667">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-667">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-668">I följande exempel beräknas COS för den angivna vinkeln.</span><span class="sxs-lookup"><span data-stu-id="f7992-668">The following example calculates the COS of the specified angle.</span></span>  
  
```  
SELECT COS(14.78)  
```  
  
 <span data-ttu-id="f7992-669">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-669">Here is the result set.</span></span>  
  
```  
[{"$1": -0.59946542619465426}]  
```  
  
####  <span data-ttu-id="f7992-670"><a name="bk_cot"></a>BET</span><span class="sxs-lookup"><span data-stu-id="f7992-670"><a name="bk_cot"></a> COT</span></span>  
 <span data-ttu-id="f7992-671">Returnerar trigonometriska värden cotangens för den angivna vinkeln i radianer i uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-671">Returns the trigonometric cotangent of the specified angle, in radians, in the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-672">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-672">**Syntax**</span></span>  
  
```  
COT(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-673">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-673">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-674">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-674">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-675">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-675">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-676">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-676">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-677">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-677">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-678">I följande exempel beräknas bet för den angivna vinkeln.</span><span class="sxs-lookup"><span data-stu-id="f7992-678">The following example calculates the COT of the specified angle.</span></span>  
  
```  
SELECT COT(124.1332)  
```  
  
 <span data-ttu-id="f7992-679">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-679">Here is the result set.</span></span>  
  
```  
[{"$1": -0.040311998371148884}]  
```  
  
####  <span data-ttu-id="f7992-680"><a name="bk_degrees"></a>GRADER</span><span class="sxs-lookup"><span data-stu-id="f7992-680"><a name="bk_degrees"></a> DEGREES</span></span>  
 <span data-ttu-id="f7992-681">Returnerar en motsvarande vinkel i grader för en vinkel angiven i radianer.</span><span class="sxs-lookup"><span data-stu-id="f7992-681">Returns the corresponding angle in degrees for an angle specified in radians.</span></span>  
  
 <span data-ttu-id="f7992-682">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-682">**Syntax**</span></span>  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-683">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-683">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-684">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-684">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-685">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-685">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-686">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-686">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-687">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-687">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-688">I följande exempel returnerar antalet grader i vinkeln i radianer PI/2.</span><span class="sxs-lookup"><span data-stu-id="f7992-688">The following example returns the number of degrees in an angle of PI/2 radians.</span></span>  
  
```  
SELECT DEGREES(PI()/2)  
```  
  
 <span data-ttu-id="f7992-689">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-689">Here is the result set.</span></span>  
  
```  
[{"$1": 90}]  
```  
  
####  <span data-ttu-id="f7992-690"><a name="bk_floor"></a>VÅNING</span><span class="sxs-lookup"><span data-stu-id="f7992-690"><a name="bk_floor"></a> FLOOR</span></span>  
 <span data-ttu-id="f7992-691">Returnerar det största heltalet mindre än eller lika med det angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-691">Returns the largest integer less than or equal to the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-692">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-692">**Syntax**</span></span>  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-693">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-693">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-694">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-694">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-695">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-695">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-696">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-696">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-697">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-697">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-698">I följande exempel visas positivt numeriskt och negativa noll värden med funktionen VÅNING.</span><span class="sxs-lookup"><span data-stu-id="f7992-698">The following example shows positive numeric, negative, and zero values with the FLOOR function.</span></span>  
  
```  
SELECT FLOOR(123.45), FLOOR(-123.45), FLOOR(0.0)  
```  
  
 <span data-ttu-id="f7992-699">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-699">Here is the result set.</span></span>  
  
```  
[{$1: 123, $2: -124, $3: 0}]  
```  
  
####  <span data-ttu-id="f7992-700"><a name="bk_exp"></a>EXP</span><span class="sxs-lookup"><span data-stu-id="f7992-700"><a name="bk_exp"></a> EXP</span></span>  
 <span data-ttu-id="f7992-701">Returnerar exponentialfördelningen värdet för det angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-701">Returns the exponential value of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-702">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-702">**Syntax**</span></span>  
  
```  
EXP (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-703">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-703">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-704">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-704">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-705">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-705">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-706">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-706">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-707">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-707">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-708">Konstanten **e** (2.718281...) är basen för den naturliga logaritmen.</span><span class="sxs-lookup"><span data-stu-id="f7992-708">The constant **e** (2.718281…), is the base of natural logarithms.</span></span>  
  
 <span data-ttu-id="f7992-709">E upphöjt till ett tal är konstanten **e** upphöjt till nummer.</span><span class="sxs-lookup"><span data-stu-id="f7992-709">The exponent of a number is the constant **e** raised to the power of the number.</span></span> <span data-ttu-id="f7992-710">Till exempel EXP(1.0) = e ^ 1.0 = 2.71828182845905 och EXP(10) = e ^ 10 = 22026.4657948067.</span><span class="sxs-lookup"><span data-stu-id="f7992-710">For example EXP(1.0) = e^1.0 = 2.71828182845905 and EXP(10) = e^10 = 22026.4657948067.</span></span>  
  
 <span data-ttu-id="f7992-711">Exponenten för den naturliga logaritmen för ett tal är antalet sig själv: EXP (loggning (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="f7992-711">The exponential of the natural logarithm of a number is the number itself: EXP (LOG (n)) = n.</span></span> <span data-ttu-id="f7992-712">Och den naturliga logaritmen för e upphöjt till ett tal är antalet sig själv: LOGG (EXP (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="f7992-712">And the natural logarithm of the exponential of a number is the number itself: LOG (EXP (n)) = n.</span></span>  
  
 <span data-ttu-id="f7992-713">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-713">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-714">I följande exempel deklarerar en variabel och returnerar exponentialfördelningen värdet för den angivna variabeln (10).</span><span class="sxs-lookup"><span data-stu-id="f7992-714">The following example declares a variable and returns the exponential value of the specified variable (10).</span></span>  
  
```  
SELECT EXP(10)  
```  
  
 <span data-ttu-id="f7992-715">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-715">Here is the result set.</span></span>  
  
```  
[{$1: 22026.465794806718}]  
```  
  
 <span data-ttu-id="f7992-716">I följande exempel returnerar exponentialfördelningen värdet av natural logarithm 20 och den naturliga logaritmen för e upphöjt till 20.</span><span class="sxs-lookup"><span data-stu-id="f7992-716">The following example returns the exponential value of the natural logarithm of 20 and the natural logarithm of the exponential of 20.</span></span> <span data-ttu-id="f7992-717">Eftersom dessa funktioner är inverterade funktioner från varandra, är det returnera värdet med avrundning för flytande punkt beräkningar i båda fallen 20.</span><span class="sxs-lookup"><span data-stu-id="f7992-717">Because these functions are inverse functions of one another, the return value with rounding for floating point math in both cases is 20.</span></span>  
  
```  
SELECT EXP(LOG(20)), LOG(EXP(20))  
```  
  
 <span data-ttu-id="f7992-718">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-718">Here is the result set.</span></span>  
  
```  
[{$1: 19.999999999999996, $2: 20}]  
```  
  
####  <span data-ttu-id="f7992-719"><a name="bk_log"></a>LOGG</span><span class="sxs-lookup"><span data-stu-id="f7992-719"><a name="bk_log"></a> LOG</span></span>  
 <span data-ttu-id="f7992-720">Returnerar den naturliga logaritmen för uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-720">Returns the natural logarithm of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-721">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-721">**Syntax**</span></span>  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 <span data-ttu-id="f7992-722">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-722">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-723">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-723">Is a numeric expression.</span></span>  
  
-   `base`  
  
     <span data-ttu-id="f7992-724">Valfritt numeriskt argument som anger basen för logaritmen.</span><span class="sxs-lookup"><span data-stu-id="f7992-724">Optional numeric argument that sets the base for the logarithm.</span></span>  
  
 <span data-ttu-id="f7992-725">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-725">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-726">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-726">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-727">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-727">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-728">Som standard returnerar LOG() den naturliga logaritmen.</span><span class="sxs-lookup"><span data-stu-id="f7992-728">By default, LOG() returns the natural logarithm.</span></span> <span data-ttu-id="f7992-729">Du kan ändra logaritmens bas till ett annat värde med hjälp av parametern bas.</span><span class="sxs-lookup"><span data-stu-id="f7992-729">You can change the base of the logarithm to another value by using the optional base parameter.</span></span>  
  
 <span data-ttu-id="f7992-730">Den naturliga logaritmen är logaritmen för talet **e**, där **e** motsvarar en onormal konstant cirka 2.718281828.</span><span class="sxs-lookup"><span data-stu-id="f7992-730">The natural logarithm is the logarithm to the base **e**, where **e** is an irrational constant approximately equal to 2.718281828.</span></span>  
  
 <span data-ttu-id="f7992-731">Den naturliga logaritmen för e upphöjt till ett tal är antalet sig själv: LOGG (EXP (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="f7992-731">The natural logarithm of the exponential of a number is the number itself: LOG( EXP( n ) ) = n.</span></span> <span data-ttu-id="f7992-732">Och exponenten för den naturliga logaritmen för ett tal är antalet sig själv: EXP (loggning (n)) = n.</span><span class="sxs-lookup"><span data-stu-id="f7992-732">And the exponential of the natural logarithm of a number is the number itself: EXP( LOG( n ) ) = n.</span></span>  
  
 <span data-ttu-id="f7992-733">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-733">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-734">I följande exempel deklarerar en variabel och Returnerar logaritmen värdet för den angivna variabeln (10).</span><span class="sxs-lookup"><span data-stu-id="f7992-734">The following example declares a variable and returns the logarithm value of the specified variable (10).</span></span>  
  
```  
SELECT LOG(10)  
```  
  
 <span data-ttu-id="f7992-735">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-735">Here is the result set.</span></span>  
  
```  
[{$1: 2.3025850929940459}]  
```  
  
 <span data-ttu-id="f7992-736">I följande exempel beräknar LOGGEN för e upphöjt till ett tal.</span><span class="sxs-lookup"><span data-stu-id="f7992-736">The following example calculates the LOG for the exponent of a number.</span></span>  
  
```  
SELECT EXP(LOG(10))  
```  
  
 <span data-ttu-id="f7992-737">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-737">Here is the result set.</span></span>  
  
```  
[{$1: 10.000000000000002}]  
```  
  
####  <span data-ttu-id="f7992-738"><a name="bk_log10"></a>LOG10</span><span class="sxs-lookup"><span data-stu-id="f7992-738"><a name="bk_log10"></a> LOG10</span></span>  
 <span data-ttu-id="f7992-739">Returnerar 10-logaritmen av uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-739">Returns the base-10 logarithm of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-740">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-740">**Syntax**</span></span>  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-741">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-741">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-742">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-742">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-743">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-743">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-744">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-744">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-745">**Kommentarer**</span><span class="sxs-lookup"><span data-stu-id="f7992-745">**Remarks**</span></span>  
  
 <span data-ttu-id="f7992-746">Funktionerna LOG10 och POWER är omvänt relaterade till varandra.</span><span class="sxs-lookup"><span data-stu-id="f7992-746">The LOG10 and POWER functions are inversely related to one another.</span></span> <span data-ttu-id="f7992-747">Till exempel 10 ^ LOG10(n) = n.</span><span class="sxs-lookup"><span data-stu-id="f7992-747">For example, 10 ^ LOG10(n) = n.</span></span>  
  
 <span data-ttu-id="f7992-748">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-748">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-749">I följande exempel deklarerar en variabel och returnerar LOG10 värdet för den angivna variabeln (100).</span><span class="sxs-lookup"><span data-stu-id="f7992-749">The following example declares a variable and returns the LOG10 value of the specified variable (100).</span></span>  
  
```  
SELECT LOG10(100)  
```  
  
 <span data-ttu-id="f7992-750">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-750">Here is the result set.</span></span>  
  
```  
[{$1: 2}]  
```  
  
####  <span data-ttu-id="f7992-751"><a name="bk_pi"></a>PI</span><span class="sxs-lookup"><span data-stu-id="f7992-751"><a name="bk_pi"></a> PI</span></span>  
 <span data-ttu-id="f7992-752">Returnerar värdet PI konstant.</span><span class="sxs-lookup"><span data-stu-id="f7992-752">Returns the constant value of PI.</span></span>  
  
 <span data-ttu-id="f7992-753">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-753">**Syntax**</span></span>  
  
```  
PI ()  
```  
  
 <span data-ttu-id="f7992-754">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-754">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-755">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-755">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-756">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-756">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-757">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-757">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-758">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-758">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-759">I följande exempel returnerar värdet PI.</span><span class="sxs-lookup"><span data-stu-id="f7992-759">The following example returns the value of PI.</span></span>  
  
```  
SELECT PI()  
```  
  
 <span data-ttu-id="f7992-760">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-760">Here is the result set.</span></span>  
  
```  
[{"$1": 3.1415926535897931}]  
```  
  
####  <span data-ttu-id="f7992-761"><a name="bk_power"></a>POWER</span><span class="sxs-lookup"><span data-stu-id="f7992-761"><a name="bk_power"></a> POWER</span></span>  
 <span data-ttu-id="f7992-762">Returnerar värdet för det angivna uttrycket till angiven potens.</span><span class="sxs-lookup"><span data-stu-id="f7992-762">Returns the value of the specified expression to the specified power.</span></span>  
  
 <span data-ttu-id="f7992-763">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-763">**Syntax**</span></span>  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 <span data-ttu-id="f7992-764">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-764">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-765">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-765">Is a numeric expression.</span></span>  
  
-   `y`  
  
     <span data-ttu-id="f7992-766">Är exponenten som du vill höja `numeric_expression`.</span><span class="sxs-lookup"><span data-stu-id="f7992-766">Is the power to which to raise `numeric_expression`.</span></span>  
  
 <span data-ttu-id="f7992-767">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-767">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-768">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-768">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-769">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-769">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-770">Exemplet nedan visar att ett tal upphöjt till 3 (kub nummer).</span><span class="sxs-lookup"><span data-stu-id="f7992-770">The following example demonstrates raising a number to the power of 3 (the cube of the number).</span></span>  
  
```  
SELECT POWER(2, 3), POWER(2.5, 3)  
```  
  
 <span data-ttu-id="f7992-771">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-771">Here is the result set.</span></span>  
  
```  
[{$1: 8, $2: 15.625}]  
```  
  
####  <span data-ttu-id="f7992-772"><a name="bk_radians"></a>RADIANER</span><span class="sxs-lookup"><span data-stu-id="f7992-772"><a name="bk_radians"></a> RADIANS</span></span>  
 <span data-ttu-id="f7992-773">Returnerar radianer när ett numeriskt uttryck i grader anges.</span><span class="sxs-lookup"><span data-stu-id="f7992-773">Returns radians when a numeric expression, in degrees, is entered.</span></span>  
  
 <span data-ttu-id="f7992-774">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-774">**Syntax**</span></span>  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-775">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-775">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-776">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-776">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-777">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-777">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-778">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-778">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-779">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-779">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-780">I följande exempel tar några vinklar som indata och returnerar motsvarande radian värden.</span><span class="sxs-lookup"><span data-stu-id="f7992-780">The following example takes a few angles as input and returns their corresponding radian values.</span></span>  
  
```  
SELECT RADIANS(-45.01), RADIANS(-181.01), RADIANS(0), RADIANS(0.1472738), RADIANS(197.1099392)  
```  
  
 <span data-ttu-id="f7992-781">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-781">Here is the result set.</span></span>  
  
```  
[{  
       "$1": -0.7855726963226477,  
       "$2": -3.1592204790349356,  
       "$3": 0,  
       "$4": 0.0025704127119236249,  
       "$5": 3.4402174274458375  
   }]  
```  
  
####  <span data-ttu-id="f7992-782"><a name="bk_round"></a>AVRUNDA</span><span class="sxs-lookup"><span data-stu-id="f7992-782"><a name="bk_round"></a> ROUND</span></span>  
 <span data-ttu-id="f7992-783">Returnerar ett tal avrundat till närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="f7992-783">Returns a numeric value, rounded to the closest integer value.</span></span>  
  
 <span data-ttu-id="f7992-784">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-784">**Syntax**</span></span>  
  
```  
ROUND(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-785">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-785">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-786">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-786">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-787">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-787">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-788">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-788">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-789">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-789">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-790">I följande exempel Avrundar följande positiva och negativa tal till närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="f7992-790">The following example rounds the following positive and negative numbers to the nearest integer.</span></span>  
  
```  
SELECT ROUND(2.4), ROUND(2.6), ROUND(2.5), ROUND(-2.4), ROUND(-2.6)  
```  
  
 <span data-ttu-id="f7992-791">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-791">Here is the result set.</span></span>  
  
```  
[{$1: 2, $2: 3, $3: 3, $4: -2, $5: -3}]  
```  
  
####  <span data-ttu-id="f7992-792"><a name="bk_sign"></a>LOGGA</span><span class="sxs-lookup"><span data-stu-id="f7992-792"><a name="bk_sign"></a> SIGN</span></span>  
 <span data-ttu-id="f7992-793">Returnerar positivt (+ 1), noll (0) eller minustecken (-1) för det angivna numeriskt uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-793">Returns the positive (+1), zero (0), or negative (-1) sign of the specified numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-794">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-794">**Syntax**</span></span>  
  
```  
SIGN(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-795">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-795">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-796">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-796">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-797">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-797">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-798">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-798">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-799">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-799">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-800">I följande exempel returnerar LOGGA värdena av talen från -2 till 2.</span><span class="sxs-lookup"><span data-stu-id="f7992-800">The following example returns the SIGN values of numbers from -2 to 2.</span></span>  
  
```  
SELECT SIGN(-2), SIGN(-1), SIGN(0), SIGN(1), SIGN(2)  
```  
  
 <span data-ttu-id="f7992-801">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-801">Here is the result set.</span></span>  
  
```  
[{$1: -1, $2: -1, $3: 0, $4: 1, $5: 1}]  
```  
  
####  <span data-ttu-id="f7992-802"><a name="bk_sin"></a>SIN</span><span class="sxs-lookup"><span data-stu-id="f7992-802"><a name="bk_sin"></a> SIN</span></span>  
 <span data-ttu-id="f7992-803">Returnerar trigonometriska värden sinus för den angivna vinkeln i radianer i det angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-803">Returns the trigonometric sine of the specified angle, in radians, in the specified expression.</span></span>  
  
 <span data-ttu-id="f7992-804">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-804">**Syntax**</span></span>  
  
```  
SIN(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-805">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-805">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-806">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-806">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-807">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-807">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-808">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-808">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-809">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-809">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-810">I följande exempel beräknas SIN för den angivna vinkeln.</span><span class="sxs-lookup"><span data-stu-id="f7992-810">The following example calculates the SIN of the specified angle.</span></span>  
  
```  
SELECT SIN(45.175643)  
```  
  
 <span data-ttu-id="f7992-811">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-811">Here is the result set.</span></span>  
  
```  
[{"$1": 0.929607286611012}]  
```  
  
####  <span data-ttu-id="f7992-812"><a name="bk_sqrt"></a>ROT</span><span class="sxs-lookup"><span data-stu-id="f7992-812"><a name="bk_sqrt"></a> SQRT</span></span>  
 <span data-ttu-id="f7992-813">Returnerar kvadratroten av det angivna numeriskt värdet.</span><span class="sxs-lookup"><span data-stu-id="f7992-813">Returns the square root of the specified numeric value.</span></span>  
  
 <span data-ttu-id="f7992-814">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-814">**Syntax**</span></span>  
  
```  
SQRT(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-815">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-815">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-816">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-816">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-817">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-817">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-818">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-818">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-819">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-819">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-820">I följande exempel returnerar kvadraten rötter av talen 1-3.</span><span class="sxs-lookup"><span data-stu-id="f7992-820">The following example returns the square roots of numbers 1-3.</span></span>  
  
```  
SELECT SQRT(1), SQRT(2.0), SQRT(3)  
```  
  
 <span data-ttu-id="f7992-821">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-821">Here is the result set.</span></span>  
  
```  
[{$1: 1, $2: 1.4142135623730952, $3: 1.7320508075688772}]  
```  
  
####  <span data-ttu-id="f7992-822"><a name="bk_square"></a>RUTA</span><span class="sxs-lookup"><span data-stu-id="f7992-822"><a name="bk_square"></a> SQUARE</span></span>  
 <span data-ttu-id="f7992-823">Returnerar kvadratroten av det angivna numeriska värdet.</span><span class="sxs-lookup"><span data-stu-id="f7992-823">Returns the square of the specified numeric value.</span></span>  
  
 <span data-ttu-id="f7992-824">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-824">**Syntax**</span></span>  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-825">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-825">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-826">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-826">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-827">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-827">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-828">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-828">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-829">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-829">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-830">I följande exempel returnerar kvadraten av talen 1-3.</span><span class="sxs-lookup"><span data-stu-id="f7992-830">The following example returns the squares of numbers 1-3.</span></span>  
  
```  
SELECT SQUARE(1), SQUARE(2.0), SQUARE(3)  
```  
  
 <span data-ttu-id="f7992-831">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-831">Here is the result set.</span></span>  
  
```  
[{$1: 1, $2: 4, $3: 9}]  
```  
  
####  <span data-ttu-id="f7992-832"><a name="bk_tan"></a>TAN</span><span class="sxs-lookup"><span data-stu-id="f7992-832"><a name="bk_tan"></a> TAN</span></span>  
 <span data-ttu-id="f7992-833">Returnerar tangens för den angivna vinkeln i radianer i det angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-833">Returns the tangent of the specified angle, in radians, in the specified expression.</span></span>  
  
 <span data-ttu-id="f7992-834">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-834">**Syntax**</span></span>  
  
```  
TAN (<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-835">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-835">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-836">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-836">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-837">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-837">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-838">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-838">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-839">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-839">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-840">I följande exempel beräknas tangens för PI () / 2.</span><span class="sxs-lookup"><span data-stu-id="f7992-840">The following example calculates the tangent of PI()/2.</span></span>  
  
```  
SELECT TAN(PI()/2);  
```  
  
 <span data-ttu-id="f7992-841">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-841">Here is the result set.</span></span>  
  
```  
[{"$1": 16331239353195370 }]  
```  
  
####  <span data-ttu-id="f7992-842"><a name="bk_trunc"></a>AVKORTA</span><span class="sxs-lookup"><span data-stu-id="f7992-842"><a name="bk_trunc"></a> TRUNC</span></span>  
 <span data-ttu-id="f7992-843">Returnerar ett numeriskt värde trunkeras till närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="f7992-843">Returns a numeric value, truncated to the closest integer value.</span></span>  
  
 <span data-ttu-id="f7992-844">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-844">**Syntax**</span></span>  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 <span data-ttu-id="f7992-845">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-845">**Arguments**</span></span>  
  
-   `numeric_expression`  
  
     <span data-ttu-id="f7992-846">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-846">Is a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-847">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-847">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-848">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-848">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-849">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-849">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-850">I följande exempel trunkerar följande positiva och negativa tal till närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="f7992-850">The following example truncates the following positive and negative numbers to the nearest integer value.</span></span>  
  
```  
SELECT TRUNC(2.4), TRUNC(2.6), TRUNC(2.5), TRUNC(-2.4), TRUNC(-2.6)  
```  
  
 <span data-ttu-id="f7992-851">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-851">Here is the result set.</span></span>  
  
```  
[{$1: 2, $2: 2, $3: 2, $4: -2, $5: -2}]  
```  
  
###  <span data-ttu-id="f7992-852"><a name="bk_type_checking_functions"></a>Ange kontrollerar funktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-852"><a name="bk_type_checking_functions"></a> Type checking functions</span></span>  
 <span data-ttu-id="f7992-853">Följande funktioner stöder typkontroll mot indatavärden och varje returnera ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-853">The following functions support type checking against input values, and each return a Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="f7992-854">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="f7992-854">IS_ARRAY</span></span>](#bk_is_array)|[<span data-ttu-id="f7992-855">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="f7992-855">IS_BOOL</span></span>](#bk_is_bool)|[<span data-ttu-id="f7992-856">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="f7992-856">IS_DEFINED</span></span>](#bk_is_defined)|  
|[<span data-ttu-id="f7992-857">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="f7992-857">IS_NULL</span></span>](#bk_is_null)|[<span data-ttu-id="f7992-858">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="f7992-858">IS_NUMBER</span></span>](#bk_is_number)|[<span data-ttu-id="f7992-859">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="f7992-859">IS_OBJECT</span></span>](#bk_is_object)|  
|[<span data-ttu-id="f7992-860">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="f7992-860">IS_PRIMITIVE</span></span>](#bk_is_primitive)|[<span data-ttu-id="f7992-861">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="f7992-861">IS_STRING</span></span>](#bk_is_string)||  
  
####  <span data-ttu-id="f7992-862"><a name="bk_is_array"></a>IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="f7992-862"><a name="bk_is_array"></a> IS_ARRAY</span></span>  
 <span data-ttu-id="f7992-863">Returnerar ett booleskt värde som anger om det angivna uttrycket är en matris.</span><span class="sxs-lookup"><span data-stu-id="f7992-863">Returns a Boolean value indicating if the type of the specified expression is an array.</span></span>  
  
 <span data-ttu-id="f7992-864">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-864">**Syntax**</span></span>  
  
```  
IS_ARRAY(<expression>)  
```  
  
 <span data-ttu-id="f7992-865">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-865">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-866">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-866">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-867">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-867">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-868">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-868">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-869">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-869">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-870">I följande exempel kontrollerar objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hjälp av funktionen IS_ARRAY.</span><span class="sxs-lookup"><span data-stu-id="f7992-870">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_ARRAY function.</span></span>  
  
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
  
 <span data-ttu-id="f7992-871">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-871">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: false, $6: true}]  
```  
  
####  <span data-ttu-id="f7992-872"><a name="bk_is_bool"></a>IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="f7992-872"><a name="bk_is_bool"></a> IS_BOOL</span></span>  
 <span data-ttu-id="f7992-873">Returnerar ett booleskt värde som anger om det angivna uttrycket är ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-873">Returns a Boolean value indicating if the type of the specified expression is a Boolean.</span></span>  
  
 <span data-ttu-id="f7992-874">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-874">**Syntax**</span></span>  
  
```  
IS_BOOL(<expression>)  
```  
  
 <span data-ttu-id="f7992-875">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-875">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-876">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-876">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-877">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-877">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-878">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-878">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-879">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-879">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-880">I följande exempel kontrollerar objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hjälp av funktionen IS_BOOL.</span><span class="sxs-lookup"><span data-stu-id="f7992-880">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_BOOL function.</span></span>  
  
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
  
 <span data-ttu-id="f7992-881">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-881">Here is the result set.</span></span>  
  
```  
[{$1: true, $2: false, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="f7992-882"><a name="bk_is_defined"></a>IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="f7992-882"><a name="bk_is_defined"></a> IS_DEFINED</span></span>  
 <span data-ttu-id="f7992-883">Returnerar ett booleskt värde som anger om egenskapen har tilldelats ett värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-883">Returns a Boolean indicating if the property has been assigned a value.</span></span>  
  
 <span data-ttu-id="f7992-884">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-884">**Syntax**</span></span>  
  
```  
IS_DEFINED(<expression>)  
```  
  
 <span data-ttu-id="f7992-885">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-885">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-886">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-886">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-887">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-887">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-888">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-888">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-889">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-889">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-890">I följande exempel kontrollerar förekomsten av en egenskap i det angivna JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f7992-890">The following example checks for the presence of a property within the specified JSON document.</span></span> <span data-ttu-id="f7992-891">Först returnerar SANT eftersom ”a” finns, men andra returnerar värdet false eftersom ”b” saknas.</span><span class="sxs-lookup"><span data-stu-id="f7992-891">The first returns true since "a" is present, but the second returns false since "b" is absent.</span></span>  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a), IS_DEFINED({ "a" : 5 }.b)  
```  
  
 <span data-ttu-id="f7992-892">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-892">Here is the result set.</span></span>  
  
```  
[{  
       "$1": true,    
       "$2": false   
   }]  
```  
  
####  <span data-ttu-id="f7992-893"><a name="bk_is_null"></a>IS_NULL</span><span class="sxs-lookup"><span data-stu-id="f7992-893"><a name="bk_is_null"></a> IS_NULL</span></span>  
 <span data-ttu-id="f7992-894">Returnerar ett booleskt värde som anger om typ av det angivna uttrycket är null.</span><span class="sxs-lookup"><span data-stu-id="f7992-894">Returns a Boolean value indicating if the type of the specified expression is null.</span></span>  
  
 <span data-ttu-id="f7992-895">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-895">**Syntax**</span></span>  
  
```  
IS_NULL(<expression>)  
```  
  
 <span data-ttu-id="f7992-896">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-896">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-897">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-897">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-898">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-898">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-899">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-899">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-900">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-900">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-901">I följande exempel kontrollerar objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hjälp av funktionen IS_NULL.</span><span class="sxs-lookup"><span data-stu-id="f7992-901">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="f7992-902">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-902">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: true, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="f7992-903"><a name="bk_is_number"></a>IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="f7992-903"><a name="bk_is_number"></a> IS_NUMBER</span></span>  
 <span data-ttu-id="f7992-904">Returnerar ett booleskt värde som anger om typ av det angivna uttrycket är ett tal.</span><span class="sxs-lookup"><span data-stu-id="f7992-904">Returns a Boolean value indicating if the type of the specified expression is a number.</span></span>  
  
 <span data-ttu-id="f7992-905">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-905">**Syntax**</span></span>  
  
```  
IS_NUMBER(<expression>)  
```  
  
 <span data-ttu-id="f7992-906">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-906">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-907">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-907">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-908">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-908">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-909">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-909">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-910">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-910">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-911">I följande exempel kontrollerar objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hjälp av funktionen IS_NULL.</span><span class="sxs-lookup"><span data-stu-id="f7992-911">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_NULL function.</span></span>  
  
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
  
 <span data-ttu-id="f7992-912">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-912">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: true, $3: false, $4: false, $5: false, $6: false}]  
```  
  
####  <span data-ttu-id="f7992-913"><a name="bk_is_object"></a>IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="f7992-913"><a name="bk_is_object"></a> IS_OBJECT</span></span>  
 <span data-ttu-id="f7992-914">Returnerar ett booleskt värde som anger om det angivna uttrycket är ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-914">Returns a Boolean value indicating if the type of the specified expression is a JSON object.</span></span>  
  
 <span data-ttu-id="f7992-915">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-915">**Syntax**</span></span>  
  
```  
IS_OBJECT(<expression>)  
```  
  
 <span data-ttu-id="f7992-916">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-916">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-917">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-917">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-918">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-918">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-919">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-919">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-920">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-920">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-921">I följande exempel kontrollerar objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hjälp av funktionen IS_OBJECT.</span><span class="sxs-lookup"><span data-stu-id="f7992-921">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_OBJECT function.</span></span>  
  
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
  
 <span data-ttu-id="f7992-922">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-922">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: false, $4: false, $5: true, $6: false}]  
```  
  
####  <span data-ttu-id="f7992-923"><a name="bk_is_primitive"></a>IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="f7992-923"><a name="bk_is_primitive"></a> IS_PRIMITIVE</span></span>  
 <span data-ttu-id="f7992-924">Returnerar ett booleskt värde som anger om typ av det angivna uttrycket är en primitiv (string, Boolean, numeriska eller null).</span><span class="sxs-lookup"><span data-stu-id="f7992-924">Returns a Boolean value indicating if the type of the specified expression is a primitive (string, Boolean, numeric or null).</span></span>  
  
 <span data-ttu-id="f7992-925">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-925">**Syntax**</span></span>  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 <span data-ttu-id="f7992-926">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-926">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-927">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-927">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-928">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-928">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-929">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-929">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-930">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-930">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-931">I följande exempel kontrollerar objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hjälp av funktionen IS_PRIMITIVE.</span><span class="sxs-lookup"><span data-stu-id="f7992-931">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_PRIMITIVE function.</span></span>  
  
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
  
 <span data-ttu-id="f7992-932">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-932">Here is the result set.</span></span>  
  
```  
[{"$1": true, "$2": true, "$3": true, "$4": true, "$5": false, "$6": false, "$7": false}]  
```  
  
####  <span data-ttu-id="f7992-933"><a name="bk_is_string"></a>IS_STRING</span><span class="sxs-lookup"><span data-stu-id="f7992-933"><a name="bk_is_string"></a> IS_STRING</span></span>  
 <span data-ttu-id="f7992-934">Returnerar ett booleskt värde som anger om det angivna uttrycket är en sträng.</span><span class="sxs-lookup"><span data-stu-id="f7992-934">Returns a Boolean value indicating if the type of the specified expression is a string.</span></span>  
  
 <span data-ttu-id="f7992-935">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-935">**Syntax**</span></span>  
  
```  
IS_STRING(<expression>)  
```  
  
 <span data-ttu-id="f7992-936">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-936">**Arguments**</span></span>  
  
-   `expression`  
  
     <span data-ttu-id="f7992-937">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-937">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-938">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-938">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-939">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-939">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-940">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-940">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-941">I följande exempel kontrollerar objekt JSON booleskt nummer, string, null, objekt, matris och odefinierad typer med hjälp av funktionen IS_STRING.</span><span class="sxs-lookup"><span data-stu-id="f7992-941">The following example checks objects of JSON Boolean, number, string, null, object, array and undefined types using the IS_STRING function.</span></span>  
  
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
  
 <span data-ttu-id="f7992-942">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-942">Here is the result set.</span></span>  
  
```  
[{$1: false, $2: false, $3: true, $4: false, $5: false, $6: false}]  
```  
  
###  <span data-ttu-id="f7992-943"><a name="bk_string_functions"></a>Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-943"><a name="bk_string_functions"></a> String functions</span></span>  
 <span data-ttu-id="f7992-944">Följande skalärfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-944">The following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="f7992-945">CONCAT</span><span class="sxs-lookup"><span data-stu-id="f7992-945">CONCAT</span></span>](#bk_concat)|[<span data-ttu-id="f7992-946">INNEHÅLLER</span><span class="sxs-lookup"><span data-stu-id="f7992-946">CONTAINS</span></span>](#bk_contains)|[<span data-ttu-id="f7992-947">ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="f7992-947">ENDSWITH</span></span>](#bk_endswith)|  
|[<span data-ttu-id="f7992-948">INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="f7992-948">INDEX_OF</span></span>](#bk_index_of)|[<span data-ttu-id="f7992-949">VÄNSTER</span><span class="sxs-lookup"><span data-stu-id="f7992-949">LEFT</span></span>](#bk_left)|[<span data-ttu-id="f7992-950">LÄNGD</span><span class="sxs-lookup"><span data-stu-id="f7992-950">LENGTH</span></span>](#bk_length)|  
|[<span data-ttu-id="f7992-951">LÄGRE</span><span class="sxs-lookup"><span data-stu-id="f7992-951">LOWER</span></span>](#bk_lower)|[<span data-ttu-id="f7992-952">LTRIM</span><span class="sxs-lookup"><span data-stu-id="f7992-952">LTRIM</span></span>](#bk_ltrim)|[<span data-ttu-id="f7992-953">ERSÄTT</span><span class="sxs-lookup"><span data-stu-id="f7992-953">REPLACE</span></span>](#bk_replace)|  
|[<span data-ttu-id="f7992-954">Replikera</span><span class="sxs-lookup"><span data-stu-id="f7992-954">REPLICATE</span></span>](#bk_replicate)|[<span data-ttu-id="f7992-955">OMVÄND</span><span class="sxs-lookup"><span data-stu-id="f7992-955">REVERSE</span></span>](#bk_reverse)|[<span data-ttu-id="f7992-956">HÖGER</span><span class="sxs-lookup"><span data-stu-id="f7992-956">RIGHT</span></span>](#bk_right)|  
|[<span data-ttu-id="f7992-957">RTRIM</span><span class="sxs-lookup"><span data-stu-id="f7992-957">RTRIM</span></span>](#bk_rtrim)|[<span data-ttu-id="f7992-958">STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="f7992-958">STARTSWITH</span></span>](#bk_startswith)|[<span data-ttu-id="f7992-959">DELSTRÄNGEN</span><span class="sxs-lookup"><span data-stu-id="f7992-959">SUBSTRING</span></span>](#bk_substring)|  
|[<span data-ttu-id="f7992-960">ÖVRE</span><span class="sxs-lookup"><span data-stu-id="f7992-960">UPPER</span></span>](#bk_upper)|||  
  
####  <span data-ttu-id="f7992-961"><a name="bk_concat"></a>CONCAT</span><span class="sxs-lookup"><span data-stu-id="f7992-961"><a name="bk_concat"></a> CONCAT</span></span>  
 <span data-ttu-id="f7992-962">Returnerar en sträng som är resultatet av att sammanfoga två eller flera strängvärden.</span><span class="sxs-lookup"><span data-stu-id="f7992-962">Returns a string that is the result of concatenating two or more string values.</span></span>  
  
 <span data-ttu-id="f7992-963">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-963">**Syntax**</span></span>  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 <span data-ttu-id="f7992-964">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-964">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-965">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-965">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-966">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-966">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-967">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-967">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-968">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-968">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-969">I följande exempel returnerar sammanfogad sträng av de angivna värdena.</span><span class="sxs-lookup"><span data-stu-id="f7992-969">The following example returns the concatenated string of the specified values.</span></span>  
  
```  
SELECT CONCAT("abc", "def")  
```  
  
 <span data-ttu-id="f7992-970">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-970">Here is the result set.</span></span>  
  
```  
[{"$1": "abcdef"}  
```  
  
####  <span data-ttu-id="f7992-971"><a name="bk_contains"></a>INNEHÅLLER</span><span class="sxs-lookup"><span data-stu-id="f7992-971"><a name="bk_contains"></a> CONTAINS</span></span>  
 <span data-ttu-id="f7992-972">Returnerar ett booleskt värde som anger om först stränguttryck innehåller andra.</span><span class="sxs-lookup"><span data-stu-id="f7992-972">Returns a Boolean indicating whether the first string expression contains the second.</span></span>  
  
 <span data-ttu-id="f7992-973">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-973">**Syntax**</span></span>  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="f7992-974">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-974">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-975">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-975">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-976">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-976">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-977">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-977">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-978">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-978">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-979">I följande exempel kontrollerar om ”abc” innehåller ”ab” och ”d”.</span><span class="sxs-lookup"><span data-stu-id="f7992-979">The following example checks if "abc" contains "ab" and contains "d".</span></span>  
  
```  
SELECT CONTAINS("abc", "ab"), CONTAINS("abc", "d")  
```  
  
 <span data-ttu-id="f7992-980">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-980">Here is the result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="f7992-981"><a name="bk_endswith"></a>ENDSWITH</span><span class="sxs-lookup"><span data-stu-id="f7992-981"><a name="bk_endswith"></a> ENDSWITH</span></span>  
 <span data-ttu-id="f7992-982">Returnerar ett booleskt värde som anger om först stränguttryck slutar med andra.</span><span class="sxs-lookup"><span data-stu-id="f7992-982">Returns a Boolean indicating whether the first string expression ends with the second.</span></span>  
  
 <span data-ttu-id="f7992-983">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-983">**Syntax**</span></span>  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="f7992-984">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-984">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-985">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-985">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-986">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-986">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-987">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-987">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-988">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-988">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-989">I följande exempel returneras ”abc” slutar med ”b” och ”bc”.</span><span class="sxs-lookup"><span data-stu-id="f7992-989">The following example returns the "abc" ends with "b" and "bc".</span></span>  
  
```  
SELECT ENDSWITH("abc", "b"), ENDSWITH("abc", "bc")  
```  
  
 <span data-ttu-id="f7992-990">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-990">Here is the result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="f7992-991"><a name="bk_index_of"></a>INDEX_OF</span><span class="sxs-lookup"><span data-stu-id="f7992-991"><a name="bk_index_of"></a> INDEX_OF</span></span>  
 <span data-ttu-id="f7992-992">Returnerar startpositionen för den första förekomsten av andra stränguttryck inom det första uttrycket i strängen, eller -1 om strängen inte hittas.</span><span class="sxs-lookup"><span data-stu-id="f7992-992">Returns the starting position of the first occurrence of the second string expression within the first specified string expression, or -1 if the string is not found.</span></span>  
  
 <span data-ttu-id="f7992-993">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-993">**Syntax**</span></span>  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="f7992-994">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-994">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-995">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-995">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-996">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-996">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-997">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-997">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-998">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-998">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-999">I följande exempel returnerar index för olika delsträngar inuti ”abc”.</span><span class="sxs-lookup"><span data-stu-id="f7992-999">The following example returns the index of various substrings inside "abc".</span></span>  
  
```  
SELECT INDEX_OF("abc", "ab"), INDEX_OF("abc", "b"), INDEX_OF("abc", "c")  
```  
  
 <span data-ttu-id="f7992-1000">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1000">Here is the result set.</span></span>  
  
```  
[{"$1": 0, "$2": 1, "$3": -1}]  
```  
  
####  <span data-ttu-id="f7992-1001"><a name="bk_left"></a>VÄNSTER</span><span class="sxs-lookup"><span data-stu-id="f7992-1001"><a name="bk_left"></a> LEFT</span></span>  
 <span data-ttu-id="f7992-1002">Returnerar en sträng med det angivna antalet tecken vänstra del.</span><span class="sxs-lookup"><span data-stu-id="f7992-1002">Returns the left part of a string with the specified number of characters.</span></span>  
  
 <span data-ttu-id="f7992-1003">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1003">**Syntax**</span></span>  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="f7992-1004">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1004">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1005">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1005">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="f7992-1006">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1006">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-1007">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1007">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1008">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1008">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1009">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1009">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1010">I följande exempel returneras ”abc” vänstra del för olika värden för längd.</span><span class="sxs-lookup"><span data-stu-id="f7992-1010">The following example returns the left part of "abc" for various length values.</span></span>  
  
```  
SELECT LEFT("abc", 1), LEFT("abc", 2)  
```  
  
 <span data-ttu-id="f7992-1011">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1011">Here is the result set.</span></span>  
  
```  
[{"$1": "ab", "$2": "ab"}]  
```  
  
####  <span data-ttu-id="f7992-1012"><a name="bk_length"></a>LÄNGD</span><span class="sxs-lookup"><span data-stu-id="f7992-1012"><a name="bk_length"></a> LENGTH</span></span>  
 <span data-ttu-id="f7992-1013">Returnerar antalet tecken i angivet stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1013">Returns the number of characters of the specified string expression.</span></span>  
  
 <span data-ttu-id="f7992-1014">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1014">**Syntax**</span></span>  
  
```  
LENGTH(<str_expr>)  
```  
  
 <span data-ttu-id="f7992-1015">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1015">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1016">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1016">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1017">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1017">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1018">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1018">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1019">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1019">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1020">I följande exempel returneras en strängens längd.</span><span class="sxs-lookup"><span data-stu-id="f7992-1020">The following example returns the length of a string.</span></span>  
  
```  
SELECT LENGTH("abc")  
```  
  
 <span data-ttu-id="f7992-1021">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1021">Here is the result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="f7992-1022"><a name="bk_lower"></a>LÄGRE</span><span class="sxs-lookup"><span data-stu-id="f7992-1022"><a name="bk_lower"></a> LOWER</span></span>  
 <span data-ttu-id="f7992-1023">Returnerar ett stränguttryck när versal data har konverterats till gemener.</span><span class="sxs-lookup"><span data-stu-id="f7992-1023">Returns a string expression after converting uppercase character data to lowercase.</span></span>  
  
 <span data-ttu-id="f7992-1024">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1024">**Syntax**</span></span>  
  
```  
LOWER(<str_expr>)  
```  
  
 <span data-ttu-id="f7992-1025">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1025">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1026">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1026">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1027">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1027">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1028">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1028">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1029">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1029">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1030">I följande exempel visas hur du använder lägre i en fråga.</span><span class="sxs-lookup"><span data-stu-id="f7992-1030">The following example shows how to use LOWER in a query.</span></span>  
  
```  
SELECT LOWER("Abc")  
```  
  
 <span data-ttu-id="f7992-1031">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1031">Here is the result set.</span></span>  
  
```  
[{"$1": "abc"}]  
  
```  
  
####  <span data-ttu-id="f7992-1032"><a name="bk_ltrim"></a>LTRIM</span><span class="sxs-lookup"><span data-stu-id="f7992-1032"><a name="bk_ltrim"></a> LTRIM</span></span>  
 <span data-ttu-id="f7992-1033">Returnerar ett stränguttryck när den tar bort inledande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="f7992-1033">Returns a string expression after it removes leading blanks.</span></span>  
  
 <span data-ttu-id="f7992-1034">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1034">**Syntax**</span></span>  
  
```  
LTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="f7992-1035">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1035">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1036">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1036">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1037">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1037">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1038">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1038">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1039">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1039">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1040">I följande exempel visas hur du använder LTRIM i en fråga.</span><span class="sxs-lookup"><span data-stu-id="f7992-1040">The following example shows how to use LTRIM inside a query.</span></span>  
  
```  
SELECT LTRIM("  abc"), LTRIM("abc"), LTRIM("abc   ")  
```  
  
 <span data-ttu-id="f7992-1041">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1041">Here is the result set.</span></span>  
  
```  
[{"$1": "abc", "$2": "abc", "$3": "abc   "}]  
```  
  
####  <span data-ttu-id="f7992-1042"><a name="bk_replace"></a>ERSÄTT</span><span class="sxs-lookup"><span data-stu-id="f7992-1042"><a name="bk_replace"></a> REPLACE</span></span>  
 <span data-ttu-id="f7992-1043">Ersätter alla förekomster av ett angivet strängvärde med ett annat värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1043">Replaces all occurrences of a specified string value with another string value.</span></span>  
  
 <span data-ttu-id="f7992-1044">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1044">**Syntax**</span></span>  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="f7992-1045">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1045">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1046">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1046">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1047">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1047">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1048">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1048">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1049">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1049">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1050">I följande exempel visas hur du använder Ersätt i en fråga.</span><span class="sxs-lookup"><span data-stu-id="f7992-1050">The following example shows how to use REPLACE in a query.</span></span>  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk")  
```  
  
 <span data-ttu-id="f7992-1051">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1051">Here is the result set.</span></span>  
  
```  
[{"$1": "This is a desk"}]  
```  
  
####  <span data-ttu-id="f7992-1052"><a name="bk_replicate"></a>REPLIKERA</span><span class="sxs-lookup"><span data-stu-id="f7992-1052"><a name="bk_replicate"></a> REPLICATE</span></span>  
 <span data-ttu-id="f7992-1053">Upprepar ett strängvärde till ett angivet antal gånger.</span><span class="sxs-lookup"><span data-stu-id="f7992-1053">Repeats a string value a specified number of times.</span></span>  
  
 <span data-ttu-id="f7992-1054">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1054">**Syntax**</span></span>  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="f7992-1055">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1055">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1056">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1056">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="f7992-1057">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1057">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-1058">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1058">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1059">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1059">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1060">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1060">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1061">I följande exempel visas hur du använder REPLIKERA i en fråga.</span><span class="sxs-lookup"><span data-stu-id="f7992-1061">The following example shows how to use REPLICATE in a query.</span></span>  
  
```  
SELECT REPLICATE("a", 3)  
```  
  
 <span data-ttu-id="f7992-1062">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1062">Here is the result set.</span></span>  
  
```  
[{"$1": "aaa"}]  
```  
  
####  <span data-ttu-id="f7992-1063"><a name="bk_reverse"></a>OMVÄND</span><span class="sxs-lookup"><span data-stu-id="f7992-1063"><a name="bk_reverse"></a> REVERSE</span></span>  
 <span data-ttu-id="f7992-1064">Returnerar ett strängvärde omvänd ordning.</span><span class="sxs-lookup"><span data-stu-id="f7992-1064">Returns the reverse order of a string value.</span></span>  
  
 <span data-ttu-id="f7992-1065">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1065">**Syntax**</span></span>  
  
```  
REVERSE(<str_expr>)  
```  
  
 <span data-ttu-id="f7992-1066">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1066">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1067">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1067">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1068">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1068">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1069">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1069">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1070">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1070">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1071">I följande exempel visas hur du använder OMVÄND i en fråga.</span><span class="sxs-lookup"><span data-stu-id="f7992-1071">The following example shows how to use REVERSE in a query.</span></span>  
  
```  
SELECT REVERSE("Abc")  
```  
  
 <span data-ttu-id="f7992-1072">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1072">Here is the result set.</span></span>  
  
```  
[{"$1": "cbA"}]  
```  
  
####  <span data-ttu-id="f7992-1073"><a name="bk_right"></a>HÖGER</span><span class="sxs-lookup"><span data-stu-id="f7992-1073"><a name="bk_right"></a> RIGHT</span></span>  
 <span data-ttu-id="f7992-1074">Returnerar den högra delen av en sträng med det angivna antalet tecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-1074">Returns the right part of a string with the specified number of characters.</span></span>  
  
 <span data-ttu-id="f7992-1075">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1075">**Syntax**</span></span>  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 <span data-ttu-id="f7992-1076">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1076">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1077">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1077">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="f7992-1078">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1078">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-1079">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1079">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1080">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1080">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1081">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1081">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1082">I följande exempel returneras den högra delen av ”abc” för olika värden för längd.</span><span class="sxs-lookup"><span data-stu-id="f7992-1082">The following example returns the right part of "abc" for various length values.</span></span>  
  
```  
SELECT RIGHT("abc", 1), RIGHT("abc", 2)  
```  
  
 <span data-ttu-id="f7992-1083">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1083">Here is the result set.</span></span>  
  
```  
[{"$1": "c", "$2": "bc"}]  
```  
  
####  <span data-ttu-id="f7992-1084"><a name="bk_rtrim"></a>RTRIM</span><span class="sxs-lookup"><span data-stu-id="f7992-1084"><a name="bk_rtrim"></a> RTRIM</span></span>  
 <span data-ttu-id="f7992-1085">Returnerar ett stränguttryck när den tar bort avslutande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="f7992-1085">Returns a string expression after it removes trailing blanks.</span></span>  
  
 <span data-ttu-id="f7992-1086">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1086">**Syntax**</span></span>  
  
```  
RTRIM(<str_expr>)  
```  
  
 <span data-ttu-id="f7992-1087">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1087">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1088">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1088">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1089">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1089">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1090">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1090">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1091">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1091">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1092">I följande exempel visas hur du använder RTRIM i en fråga.</span><span class="sxs-lookup"><span data-stu-id="f7992-1092">The following example shows how to use RTRIM inside a query.</span></span>  
  
```  
SELECT RTRIM("  abc"), RTRIM("abc"), RTRIM("abc   ")  
```  
  
 <span data-ttu-id="f7992-1093">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1093">Here is the result set.</span></span>  
  
```  
[{"$1": "   abc", "$2": "abc", "$3": "abc"}]  
```  
  
####  <span data-ttu-id="f7992-1094"><a name="bk_startswith"></a>STARTSWITH</span><span class="sxs-lookup"><span data-stu-id="f7992-1094"><a name="bk_startswith"></a> STARTSWITH</span></span>  
 <span data-ttu-id="f7992-1095">Returnerar ett booleskt värde som anger om först stränguttryck börjar med andra.</span><span class="sxs-lookup"><span data-stu-id="f7992-1095">Returns a Boolean indicating whether the first string expression starts with the second.</span></span>  
  
 <span data-ttu-id="f7992-1096">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1096">**Syntax**</span></span>  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 <span data-ttu-id="f7992-1097">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1097">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1098">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1098">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1099">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1099">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1100">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1100">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-1101">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1101">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1102">I följande exempel kontrollerar om strängen ”abc” börjar med ”b” och ”a”.</span><span class="sxs-lookup"><span data-stu-id="f7992-1102">The following example checks if the string "abc" begins with "b" and "a".</span></span>  
  
```  
SELECT STARTSWITH("abc", "b"), STARTSWITH("abc", "a")  
```  
  
 <span data-ttu-id="f7992-1103">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1103">Here is the result set.</span></span>  
  
```  
[{"$1": false, "$2": true}]  
```  
  
####  <span data-ttu-id="f7992-1104"><a name="bk_substring"></a>DELSTRÄNGEN</span><span class="sxs-lookup"><span data-stu-id="f7992-1104"><a name="bk_substring"></a> SUBSTRING</span></span>  
 <span data-ttu-id="f7992-1105">Returnerar en del av ett stränguttryck som börjar vid den angivna nollbaserade teckenpositionen och fortsätter med den angivna längden eller till slutet av strängen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1105">Returns part of a string expression starting at the specified character zero-based position and continues to the specified length, or to the end of the string.</span></span>  
  
 <span data-ttu-id="f7992-1106">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1106">**Syntax**</span></span>  
  
```  
SUBSTRING(<str_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="f7992-1107">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1107">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1108">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1108">Is any valid string expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="f7992-1109">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1109">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-1110">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1110">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1111">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1111">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1112">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1112">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1113">I följande exempel Returnerar delsträngen av ”abc” som börjar vid 1 och för en längd på 1 tecken.</span><span class="sxs-lookup"><span data-stu-id="f7992-1113">The following example returns the substring of "abc" starting at 1 and for a length of 1 character.</span></span>  
  
```  
SELECT SUBSTRING("abc", 1, 1)  
```  
  
 <span data-ttu-id="f7992-1114">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1114">Here is the result set.</span></span>  
  
```  
[{"$1": "b"}]  
```  
  
####  <span data-ttu-id="f7992-1115"><a name="bk_upper"></a>ÖVRE</span><span class="sxs-lookup"><span data-stu-id="f7992-1115"><a name="bk_upper"></a> UPPER</span></span>  
 <span data-ttu-id="f7992-1116">Returnerar ett stränguttryck när gemen data har konverterats till versaler.</span><span class="sxs-lookup"><span data-stu-id="f7992-1116">Returns a string expression after converting lowercase character data to uppercase.</span></span>  
  
 <span data-ttu-id="f7992-1117">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1117">**Syntax**</span></span>  
  
```  
UPPER(<str_expr>)  
```  
  
 <span data-ttu-id="f7992-1118">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1118">**Arguments**</span></span>  
  
-   `str_expr`  
  
     <span data-ttu-id="f7992-1119">Är ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1119">Is any valid string expression.</span></span>  
  
 <span data-ttu-id="f7992-1120">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1120">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1121">Returnerar ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1121">Returns a string expression.</span></span>  
  
 <span data-ttu-id="f7992-1122">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1122">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1123">I följande exempel visas hur du använder längst upp i en fråga</span><span class="sxs-lookup"><span data-stu-id="f7992-1123">The following example shows how to use UPPER in a query</span></span>  
  
```  
SELECT UPPER("Abc")  
```  
  
 <span data-ttu-id="f7992-1124">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1124">Here is the result set.</span></span>  
  
```  
[{"$1": "ABC"}]  
```  
  
###  <span data-ttu-id="f7992-1125"><a name="bk_array_functions"></a>Matrisfunktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-1125"><a name="bk_array_functions"></a> Array functions</span></span>  
 <span data-ttu-id="f7992-1126">Följande skalärfunktioner utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde</span><span class="sxs-lookup"><span data-stu-id="f7992-1126">The following scalar functions perform an operation on an array input value and return numeric, Boolean or array value</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="f7992-1127">ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="f7992-1127">ARRAY_CONCAT</span></span>](#bk_array_concat)|[<span data-ttu-id="f7992-1128">ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="f7992-1128">ARRAY_CONTAINS</span></span>](#bk_array_contains)|[<span data-ttu-id="f7992-1129">ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="f7992-1129">ARRAY_LENGTH</span></span>](#bk_array_length)|  
|[<span data-ttu-id="f7992-1130">ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="f7992-1130">ARRAY_SLICE</span></span>](#bk_array_slice)|||  
  
####  <span data-ttu-id="f7992-1131"><a name="bk_array_concat"></a>ARRAY_CONCAT</span><span class="sxs-lookup"><span data-stu-id="f7992-1131"><a name="bk_array_concat"></a> ARRAY_CONCAT</span></span>  
 <span data-ttu-id="f7992-1132">Returnerar en matris som är resultatet av sammanfogar två eller flera matrisvärden.</span><span class="sxs-lookup"><span data-stu-id="f7992-1132">Returns an array that is the result of concatenating two or more array values.</span></span>  
  
 <span data-ttu-id="f7992-1133">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1133">**Syntax**</span></span>  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 <span data-ttu-id="f7992-1134">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1134">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="f7992-1135">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1135">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="f7992-1136">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1136">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1137">Returnerar en matrisuttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1137">Returns an array expression.</span></span>  
  
 <span data-ttu-id="f7992-1138">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1138">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1139">I följande exempel så att sammanfoga två matriser.</span><span class="sxs-lookup"><span data-stu-id="f7992-1139">The following example how to concatenate two arrays.</span></span>  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"])  
```  
  
 <span data-ttu-id="f7992-1140">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1140">Here is the result set.</span></span>  
  
```  
[{"$1": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <span data-ttu-id="f7992-1141"><a name="bk_array_contains"></a>ARRAY_CONTAINS</span><span class="sxs-lookup"><span data-stu-id="f7992-1141"><a name="bk_array_contains"></a> ARRAY_CONTAINS</span></span>  
 <span data-ttu-id="f7992-1142">Returnerar ett booleskt värde som anger om matrisen innehåller det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="f7992-1142">Returns a Boolean indicating whether the array contains the specified value.</span></span>  
  
 <span data-ttu-id="f7992-1143">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1143">**Syntax**</span></span>  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr>)  
```  
  
 <span data-ttu-id="f7992-1144">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1144">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="f7992-1145">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1145">Is any valid array expression.</span></span>  
  
-   `expr`  
  
     <span data-ttu-id="f7992-1146">Är ett giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1146">Is any valid expression.</span></span>  
  
 <span data-ttu-id="f7992-1147">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1147">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1148">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1148">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="f7992-1149">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1149">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1150">I följande exempel hur du kontrollerar om medlemskap i en matris med ARRAY_CONTAINS.</span><span class="sxs-lookup"><span data-stu-id="f7992-1150">The following example how to check for membership in an array using ARRAY_CONTAINS.</span></span>  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples"),  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes")  
```  
  
 <span data-ttu-id="f7992-1151">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1151">Here is the result set.</span></span>  
  
```  
[{"$1": true, "$2": false}]  
```  
  
####  <span data-ttu-id="f7992-1152"><a name="bk_array_length"></a>ARRAY_LENGTH</span><span class="sxs-lookup"><span data-stu-id="f7992-1152"><a name="bk_array_length"></a> ARRAY_LENGTH</span></span>  
 <span data-ttu-id="f7992-1153">Returnerar antal element i matrisen uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f7992-1153">Returns the number of elements of the specified array expression.</span></span>  
  
 <span data-ttu-id="f7992-1154">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1154">**Syntax**</span></span>  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 <span data-ttu-id="f7992-1155">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1155">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="f7992-1156">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1156">Is any valid array expression.</span></span>  
  
 <span data-ttu-id="f7992-1157">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1157">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1158">Returnerar ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1158">Returns a numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-1159">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1159">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1160">I följande exempel hur du hämtar en matris med ARRAY_LENGTH längd.</span><span class="sxs-lookup"><span data-stu-id="f7992-1160">The following example how to get the length of an array using ARRAY_LENGTH.</span></span>  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"])  
```  
  
 <span data-ttu-id="f7992-1161">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1161">Here is the result set.</span></span>  
  
```  
[{"$1": 3}]  
```  
  
####  <span data-ttu-id="f7992-1162"><a name="bk_array_slice"></a>ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="f7992-1162"><a name="bk_array_slice"></a> ARRAY_SLICE</span></span>  
 <span data-ttu-id="f7992-1163">Returnerar en del av ett matrisuttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1163">Returns part of an array expression.</span></span>
  
 <span data-ttu-id="f7992-1164">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1164">**Syntax**</span></span>  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 <span data-ttu-id="f7992-1165">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1165">**Arguments**</span></span>  
  
-   `arr_expr`  
  
     <span data-ttu-id="f7992-1166">Är ett giltigt array-uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1166">Is any valid array expression.</span></span>  
  
-   `num_expr`  
  
     <span data-ttu-id="f7992-1167">Är ett numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1167">Is any valid numeric expression.</span></span>  
  
 <span data-ttu-id="f7992-1168">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1168">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1169">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1169">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="f7992-1170">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1170">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1171">I följande exempel hur du hämtar en del av en matris med ARRAY_SLICE.</span><span class="sxs-lookup"><span data-stu-id="f7992-1171">The following example how to get a part of an array using ARRAY_SLICE.</span></span>  
  
```  
SELECT   
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1),  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1)  
```  
  
 <span data-ttu-id="f7992-1172">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1172">Here is the result set.</span></span>  
  
```  
[{  
           "$1": ["strawberries", "bananas"],   
           "$2": ["strawberries"]  
       }]  
```  
  
###  <span data-ttu-id="f7992-1173"><a name="bk_spatial_functions"></a>Spatial funktioner</span><span class="sxs-lookup"><span data-stu-id="f7992-1173"><a name="bk_spatial_functions"></a> Spatial functions</span></span>  
 <span data-ttu-id="f7992-1174">Följande skalärfunktioner utföra en åtgärd på ett indatavärde Rumsobjektet och returnera ett numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1174">The following scalar functions perform an operation on an spatial object input value and return a numeric or Boolean value.</span></span>  
  
||||  
|-|-|-|  
|[<span data-ttu-id="f7992-1175">ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="f7992-1175">ST_DISTANCE</span></span>](#bk_st_distance)|[<span data-ttu-id="f7992-1176">ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="f7992-1176">ST_WITHIN</span></span>](#bk_st_within)|[<span data-ttu-id="f7992-1177">ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="f7992-1177">ST_INTERSECTS</span></span>](#bk_st_intersects)|[<span data-ttu-id="f7992-1178">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="f7992-1178">ST_ISVALID</span></span>](#bk_st_isvalid)|  
|[<span data-ttu-id="f7992-1179">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="f7992-1179">ST_ISVALIDDETAILED</span></span>](#bk_st_isvaliddetailed)|||  
  
####  <span data-ttu-id="f7992-1180"><a name="bk_st_distance"></a>ST_DISTANCE</span><span class="sxs-lookup"><span data-stu-id="f7992-1180"><a name="bk_st_distance"></a> ST_DISTANCE</span></span>  
 <span data-ttu-id="f7992-1181">Returnerar avståndet mellan de två GeoJSON punkt, Polygon eller LineString-uttrycken.</span><span class="sxs-lookup"><span data-stu-id="f7992-1181">Returns the distance between the two GeoJSON Point, Polygon, or LineString expressions.</span></span>  
  
 <span data-ttu-id="f7992-1182">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1182">**Syntax**</span></span>  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="f7992-1183">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1183">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="f7992-1184">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-1184">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="f7992-1185">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1185">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1186">Returnerar ett stränguttryck som innehåller avståndet.</span><span class="sxs-lookup"><span data-stu-id="f7992-1186">Returns a numeric expression containing the distance.</span></span> <span data-ttu-id="f7992-1187">Detta uttrycks i mätare för referens i systemet.</span><span class="sxs-lookup"><span data-stu-id="f7992-1187">This is expressed in meters for the default reference system.</span></span>  
  
 <span data-ttu-id="f7992-1188">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1188">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1189">I följande exempel visas hur du återställer alla family dokument som ligger inom 30 km för den angivna platsen med hjälp av funktionen ST_DISTANCE inbyggda.</span><span class="sxs-lookup"><span data-stu-id="f7992-1189">The following example shows how to return all family documents that are within 30 km of the specified location using the ST_DISTANCE built-in function.</span></span> <span data-ttu-id="f7992-1190">.</span><span class="sxs-lookup"><span data-stu-id="f7992-1190">.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 <span data-ttu-id="f7992-1191">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1191">Here is the result set.</span></span>  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <span data-ttu-id="f7992-1192"><a name="bk_st_within"></a>ST_WITHIN</span><span class="sxs-lookup"><span data-stu-id="f7992-1192"><a name="bk_st_within"></a> ST_WITHIN</span></span>  
 <span data-ttu-id="f7992-1193">Returnerar ett booleskt uttryck som anger om GeoJSON (Point, LineString eller Polygon) det angivna objektet i det första argumentet är i GeoJSON (Point, LineString eller Polygon) i det andra argumentet.</span><span class="sxs-lookup"><span data-stu-id="f7992-1193">Returns a Boolean expression indicating whether the GeoJSON object (Point, Polygon, or LineString) specified in the first argument is within the GeoJSON (Point, Polygon, or LineString) in the second argument.</span></span>  
  
 <span data-ttu-id="f7992-1194">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1194">**Syntax**</span></span>  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="f7992-1195">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1195">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="f7992-1196">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-1196">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="f7992-1197">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-1197">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="f7992-1198">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1198">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1199">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1199">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="f7992-1200">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1200">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1201">I följande exempel visas hur du hittar alla family dokument i en polygon med ST_WITHIN.</span><span class="sxs-lookup"><span data-stu-id="f7992-1201">The following example shows how to find all family documents within a polygon using ST_WITHIN.</span></span>  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="f7992-1202">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1202">Here is the result set.</span></span>  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <span data-ttu-id="f7992-1203"><a name="bk_st_intersects"></a>ST_INTERSECTS</span><span class="sxs-lookup"><span data-stu-id="f7992-1203"><a name="bk_st_intersects"></a> ST_INTERSECTS</span></span>  
 <span data-ttu-id="f7992-1204">Returnerar ett booleskt uttryck som anger om GeoJSON (Point, LineString eller Polygon) det angivna objektet i det första argumentet korsar GeoJSON (Point, LineString eller Polygon) i det andra argumentet.</span><span class="sxs-lookup"><span data-stu-id="f7992-1204">Returns a Boolean expression indicating whether the GeoJSON object (Point, Polygon, or LineString) specified in the first argument intersects the GeoJSON (Point, Polygon, or LineString) in the second argument.</span></span>  
  
 <span data-ttu-id="f7992-1205">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1205">**Syntax**</span></span>  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 <span data-ttu-id="f7992-1206">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1206">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="f7992-1207">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-1207">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
 
-   `spatial_expr`  
  
     <span data-ttu-id="f7992-1208">Är ett giltigt uttryck för GeoJSON punkt, Polygon eller LineString-objekt.</span><span class="sxs-lookup"><span data-stu-id="f7992-1208">Is any valid GeoJSON Point, Polygon, or LineString object expression.</span></span>  
  
 <span data-ttu-id="f7992-1209">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1209">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1210">Returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1210">Returns a Boolean value.</span></span>  
  
 <span data-ttu-id="f7992-1211">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1211">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1212">I följande exempel visas hur du hittar alla områden som korsar med angivna polygonen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1212">The following example shows how to find all areas that intersects with the given polygon.</span></span>  
  
```  
SELECT a.id   
FROM Areas a   
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 <span data-ttu-id="f7992-1213">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1213">Here is the result set.</span></span>  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <span data-ttu-id="f7992-1214"><a name="bk_st_isvalid"></a>ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="f7992-1214"><a name="bk_st_isvalid"></a> ST_ISVALID</span></span>  
 <span data-ttu-id="f7992-1215">Returnerar ett booleskt värde som anger om det angivna uttrycket GeoJSON punkt, Polygon eller LineString är giltig.</span><span class="sxs-lookup"><span data-stu-id="f7992-1215">Returns a Boolean value indicating whether the specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span>  
  
 <span data-ttu-id="f7992-1216">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1216">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="f7992-1217">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1217">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="f7992-1218">Är ett giltigt GeoJSON punkt, Polygon eller LineString uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1218">Is any valid GeoJSON Point, Polygon, or LineString expression.</span></span>  
  
 <span data-ttu-id="f7992-1219">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1219">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1220">Returnerar ett booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1220">Returns a Boolean expression.</span></span>  
  
 <span data-ttu-id="f7992-1221">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1221">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1222">I följande exempel visas hur du kontrollerar om en är giltig med ST_VALID.</span><span class="sxs-lookup"><span data-stu-id="f7992-1222">The following example shows how to check if a point is valid using ST_VALID.</span></span>  
  
 <span data-ttu-id="f7992-1223">Den här platsen har till exempel ett latitud-värde som inte är i det giltiga värdeintervallet [-90, 90] så frågan returnerar false.</span><span class="sxs-lookup"><span data-stu-id="f7992-1223">For example, this point has a latitude value that's not in the valid range of values [-90, 90], so the query returns false.</span></span>  
  
 <span data-ttu-id="f7992-1224">För polygoner kräver GeoJSON-specifikationen att senaste koordinaten paret som angetts ska vara samma som först som skapar en sluten form.</span><span class="sxs-lookup"><span data-stu-id="f7992-1224">For polygons, the GeoJSON specification requires that the last coordinate pair provided should be the same as the first, to create a closed shape.</span></span> <span data-ttu-id="f7992-1225">Punkter i en polygon måste anges i följd medurs ordning.</span><span class="sxs-lookup"><span data-stu-id="f7992-1225">Points within a polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="f7992-1226">En polygon som angetts i medurs ordning representerar inversen till regionen i den.</span><span class="sxs-lookup"><span data-stu-id="f7992-1226">A polygon specified in clockwise order represents the inverse of the region within it.</span></span>  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })  
```  
  
 <span data-ttu-id="f7992-1227">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1227">Here is the result set.</span></span>  
  
```  
[{ "$1": false }]  
```  
  
####  <span data-ttu-id="f7992-1228"><a name="bk_st_isvaliddetailed"></a>ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="f7992-1228"><a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED</span></span>  
 <span data-ttu-id="f7992-1229">Returnerar ett JSON-värde som innehåller ett booleskt värde värdet om det angivna uttrycket för GeoJSON punkt, Polygon eller LineString är giltiga och ogiltiga, dessutom orsak som ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1229">Returns a JSON value containing a Boolean value if the specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally the reason as a string value.</span></span>  
  
 <span data-ttu-id="f7992-1230">**Syntax**</span><span class="sxs-lookup"><span data-stu-id="f7992-1230">**Syntax**</span></span>  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 <span data-ttu-id="f7992-1231">**Argument**</span><span class="sxs-lookup"><span data-stu-id="f7992-1231">**Arguments**</span></span>  
  
-   `spatial_expr`  
  
     <span data-ttu-id="f7992-1232">Är ett giltigt GeoJSON punkt eller polygon uttryck.</span><span class="sxs-lookup"><span data-stu-id="f7992-1232">Is any valid GeoJSON point or polygon expression.</span></span>  
  
 <span data-ttu-id="f7992-1233">**Returtyper**</span><span class="sxs-lookup"><span data-stu-id="f7992-1233">**Return Types**</span></span>  
  
 <span data-ttu-id="f7992-1234">Returnerar ett JSON-värde som innehåller ett booleskt värde värdet om det angivna GeoJSON punkt eller polygon uttrycket är giltigt och ogiltig, dessutom orsak som ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="f7992-1234">Returns a JSON value containing a Boolean value if the specified GeoJSON point or polygon expression is valid, and if invalid, additionally the reason as a string value.</span></span>  
  
 <span data-ttu-id="f7992-1235">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="f7992-1235">**Examples**</span></span>  
  
 <span data-ttu-id="f7992-1236">I följande exempel kontrollera giltigheten (med information) med hjälp av ST_ISVALIDDETAILED.</span><span class="sxs-lookup"><span data-stu-id="f7992-1236">The following example how to check validity (with details) using ST_ISVALIDDETAILED.</span></span>  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
})  
```  
  
 <span data-ttu-id="f7992-1237">Här är resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f7992-1237">Here is the result set.</span></span>  
  
```  
[{  
  "$1": {   
    "valid": false,   
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  
  
## <a name="next-steps"></a><span data-ttu-id="f7992-1238">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7992-1238">Next steps</span></span>  
 <span data-ttu-id="f7992-1239">[SQL-syntax och SQL-fråga för Azure Cosmos DB](documentdb-sql-query.md) </span><span class="sxs-lookup"><span data-stu-id="f7992-1239">[SQL syntax and SQL query for Azure Cosmos DB](documentdb-sql-query.md) </span></span>  
 [<span data-ttu-id="f7992-1240">Azure DB Cosmos-dokumentation</span><span class="sxs-lookup"><span data-stu-id="f7992-1240">Azure Cosmos DB documentation</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/)  
  
  
