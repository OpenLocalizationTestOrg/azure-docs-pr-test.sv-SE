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
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="ec284-105">SQL-frågor för Azure Cosmos DB DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="ec284-105">SQL queries for Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="ec284-106">Microsoft Azure Cosmos DB stöder förfrågningar till dokument med SQL (Structured Query Language) som ett JSON-frågespråk.</span><span class="sxs-lookup"><span data-stu-id="ec284-106">Microsoft Azure Cosmos DB supports querying documents using SQL (Structured Query Language) as a JSON query language.</span></span> <span data-ttu-id="ec284-107">Cosmos DB är verkligen schemafria.</span><span class="sxs-lookup"><span data-stu-id="ec284-107">Cosmos DB is truly schema-free.</span></span> <span data-ttu-id="ec284-108">Tack vare dess åtagande toohello JSON-datamodell direkt i databasmotorn hello ger automatisk indexering av JSON-dokument utan explicita schema eller att sekundärindex.</span><span class="sxs-lookup"><span data-stu-id="ec284-108">By virtue of its commitment toohello JSON data model directly within hello database engine, it provides automatic indexing of JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> 

<span data-ttu-id="ec284-109">När du utformar hello frågespråk för Cosmos DB hade vi två mål i åtanke:</span><span class="sxs-lookup"><span data-stu-id="ec284-109">While designing hello query language for Cosmos DB, we had two goals in mind:</span></span>

* <span data-ttu-id="ec284-110">I stället för inventing en ny JSON-frågespråket ville vi toosupport SQL.</span><span class="sxs-lookup"><span data-stu-id="ec284-110">Instead of inventing a new JSON query language, we wanted toosupport SQL.</span></span> <span data-ttu-id="ec284-111">SQL är ett av hello bekant och mest populära språk för frågan.</span><span class="sxs-lookup"><span data-stu-id="ec284-111">SQL is one of hello most familiar and popular query languages.</span></span> <span data-ttu-id="ec284-112">Cosmos-Databasens SQL är en formell programmeringsmodell för komplexa frågor via JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-112">Cosmos DB SQL provides a formal programming model for rich queries over JSON documents.</span></span>
* <span data-ttu-id="ec284-113">Som en JSON-dokument databas kan utföra JavaScript direkt i databasmotorn hello ville vi toouse JavaScript programmeringsmodell som hello foundation våra query Language.</span><span class="sxs-lookup"><span data-stu-id="ec284-113">As a JSON document database capable of executing JavaScript directly in hello database engine, we wanted toouse JavaScript's programming model as hello foundation for our query language.</span></span> <span data-ttu-id="ec284-114">Hej DocumentDB API SQL är rotad i typsystemet i Javascript's, utvärdering av uttryck och funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="ec284-114">hello DocumentDB API SQL is rooted in JavaScript's type system, expression evaluation, and function invocation.</span></span> <span data-ttu-id="ec284-115">Detta i sin tur är en fysisk programmeringsmodell för relationella projektioner, hierarkisk navigering i JSON-dokument, self kopplingar, spatial frågor och anrop av användardefinierade funktioner (UDF) helt skrivna i JavaScript, bland andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-115">This in-turn provides a natural programming model for relational projections, hierarchical navigation across JSON documents, self joins, spatial queries, and invocation of user-defined functions (UDFs) written entirely in JavaScript, among other features.</span></span> 

<span data-ttu-id="ec284-116">Vi tror att dessa funktioner är viktiga tooreducing hello friktion mellan hello programmet och hello databasen och är avgörande för utvecklarproduktivitet.</span><span class="sxs-lookup"><span data-stu-id="ec284-116">We believe that these capabilities are key tooreducing hello friction between hello application and hello database and are crucial for developer productivity.</span></span>

<span data-ttu-id="ec284-117">Vi rekommenderar att komma igång genom att titta på hello följande video, där Aravind Ramachandran visar Cosmos DB frågar funktioner, och genom att besöka vårt [Query Playground](http://www.documentdb.com/sql/demo), där du kan prova att använda Cosmos-DB och köra SQL-frågor mot vår datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="ec284-117">We recommend getting started by watching hello following video, where Aravind Ramachandran shows Cosmos DB's querying capabilities, and by visiting our [Query Playground](http://www.documentdb.com/sql/demo), where you can try out Cosmos DB and run SQL queries against our dataset.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

<span data-ttu-id="ec284-118">Gå sedan tillbaka toothis artikel, där vi börjar med en SQL-fråga självstudiekurs som vägleder dig igenom några enkla JSON-dokument och SQL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="ec284-118">Then, return toothis article, where we start with a SQL query tutorial that walks you through some simple JSON documents and SQL commands.</span></span>

## <span data-ttu-id="ec284-119"><a id="GettingStarted"></a>Komma igång med SQL-kommandon i Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ec284-119"><a id="GettingStarted"></a>Getting started with SQL commands in Cosmos DB</span></span>
<span data-ttu-id="ec284-120">toosee Cosmos-Databasens SQL på fungerar, vi börjar med några enkla JSON-dokument och gå igenom några enkla frågor mot den.</span><span class="sxs-lookup"><span data-stu-id="ec284-120">toosee Cosmos DB SQL at work, let's begin with a few simple JSON documents and walk through some simple queries against it.</span></span> <span data-ttu-id="ec284-121">Överväg att dessa två JSON-dokument om två serier.</span><span class="sxs-lookup"><span data-stu-id="ec284-121">Consider these two JSON documents about two families.</span></span> <span data-ttu-id="ec284-122">Med Cosmos DB behöver vi inte toocreate alla scheman eller sekundärindex explicit.</span><span class="sxs-lookup"><span data-stu-id="ec284-122">With Cosmos DB, we do not need toocreate any schemas or secondary indices explicitly.</span></span> <span data-ttu-id="ec284-123">Vi behöver tooinsert hello JSON-dokument tooa Cosmos DB samling och sedan fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-123">We simply need tooinsert hello JSON documents tooa Cosmos DB collection and subsequently query.</span></span> <span data-ttu-id="ec284-124">Här är ett enkelt JSON-dokumentet för hello familjen Andersen, hello överordnade, underordnade (och deras husdjur) adress och registreringsinformation.</span><span class="sxs-lookup"><span data-stu-id="ec284-124">Here we have a simple JSON document for hello Andersen family, hello parents, children (and their pets), address, and registration information.</span></span> <span data-ttu-id="ec284-125">hello dokument har strängar, siffror, booleska värden, matriser och kapslade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ec284-125">hello document has strings, numbers, Booleans, arrays, and nested properties.</span></span> 

<span data-ttu-id="ec284-126">**Dokumentet**</span><span class="sxs-lookup"><span data-stu-id="ec284-126">**Document**</span></span>  

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

<span data-ttu-id="ec284-127">Här är ett andra dokument med en skillnaden mellan – `givenName` och `familyName` används i stället för `firstName` och `lastName`.</span><span class="sxs-lookup"><span data-stu-id="ec284-127">Here's a second document with one subtle difference – `givenName` and `familyName` are used instead of `firstName` and `lastName`.</span></span>

<span data-ttu-id="ec284-128">**Dokumentet**</span><span class="sxs-lookup"><span data-stu-id="ec284-128">**Document**</span></span>  

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

<span data-ttu-id="ec284-129">Nu ska vi prova några frågor mot den här data toounderstand vissa hello nyckeln aspekter av DocumentDB API SQL.</span><span class="sxs-lookup"><span data-stu-id="ec284-129">Now let's try a few queries against this data toounderstand some of hello key aspects of DocumentDB API SQL.</span></span> <span data-ttu-id="ec284-130">Till exempel hello följande fråga returnerar hello dokument där hello id-fältet matchar `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="ec284-130">For example, hello following query returns hello documents where hello id field matches `AndersenFamily`.</span></span> <span data-ttu-id="ec284-131">Eftersom det är en `SELECT *`, hello utdata från frågan hello är hello fullständig JSON-dokumentet:</span><span class="sxs-lookup"><span data-stu-id="ec284-131">Since it's a `SELECT *`, hello output of hello query is hello complete JSON document:</span></span>

<span data-ttu-id="ec284-132">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-132">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-133">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-133">**Results**</span></span>

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


<span data-ttu-id="ec284-134">Tänk dig nu hello fallet när vi behöver tooreformat hello JSON-utdata i en annan form.</span><span class="sxs-lookup"><span data-stu-id="ec284-134">Now consider hello case where we need tooreformat hello JSON output in a different shape.</span></span> <span data-ttu-id="ec284-135">Den här frågan projekt nya JSON-objekt med två valda fält, namn och ort, när hello adress ort har hello samma namn som hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ec284-135">This query projects a new JSON object with two selected fields, Name and City, when hello address' city has hello same name as hello state.</span></span> <span data-ttu-id="ec284-136">I det här fallet matchar ”NY, NY”.</span><span class="sxs-lookup"><span data-stu-id="ec284-136">In this case, "NY, NY" matches.</span></span>

<span data-ttu-id="ec284-137">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-137">**Query**</span></span>    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

<span data-ttu-id="ec284-138">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-138">**Results**</span></span>

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


<span data-ttu-id="ec284-139">hello nästa fråga returnerar alla hello angivet namn på barn i hello familj vars id matchar `WakefieldFamily` sorterade efter hello ort där du bor.</span><span class="sxs-lookup"><span data-stu-id="ec284-139">hello next query returns all hello given names of children in hello family whose id matches `WakefieldFamily` ordered by hello city of residence.</span></span>

<span data-ttu-id="ec284-140">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-140">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

<span data-ttu-id="ec284-141">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-141">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


<span data-ttu-id="ec284-142">Vi vill gärna toodraw uppmärksamhet tooa några anmärkningsvärda aspekter av hello Cosmos-DB-frågespråket genom hello-exempel som vi har sett:</span><span class="sxs-lookup"><span data-stu-id="ec284-142">We would like toodraw attention tooa few noteworthy aspects of hello Cosmos DB query language through hello examples we've seen so far:</span></span>  

* <span data-ttu-id="ec284-143">Eftersom DocumentDB API SQL fungerar på JSON-värden, behandlar trädet Formats entiteter i stället för rader och kolumner.</span><span class="sxs-lookup"><span data-stu-id="ec284-143">Since DocumentDB API SQL works on JSON values, it deals with tree shaped entities instead of rows and columns.</span></span> <span data-ttu-id="ec284-144">Därför hello språket kan du se toonodes hello träd på varje godtycklig djup som `Node1.Node2.Node3…..Nodem`, liknande toorelational SQL refererande toohello två del referens för `<table>.<column>`.</span><span class="sxs-lookup"><span data-stu-id="ec284-144">Therefore, hello language lets you refer toonodes of hello tree at any arbitrary depth, like `Node1.Node2.Node3…..Nodem`, similar toorelational SQL referring toohello two part reference of `<table>.<column>`.</span></span>   
* <span data-ttu-id="ec284-145">hello strukturerade query language fungerar med schemat mindre data.</span><span class="sxs-lookup"><span data-stu-id="ec284-145">hello structured query language works with schema-less data.</span></span> <span data-ttu-id="ec284-146">Därför hello typen system måste toobe gränsen dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="ec284-146">Therefore, hello type system needs toobe bound dynamically.</span></span> <span data-ttu-id="ec284-147">hello kan samma uttryck ge olika typer på olika dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-147">hello same expression could yield different types on different documents.</span></span> <span data-ttu-id="ec284-148">hello resultatet av en fråga är ett giltigt JSON-värde, men är inte säkert toobe för ett fast schema.</span><span class="sxs-lookup"><span data-stu-id="ec284-148">hello result of a query is a valid JSON value, but is not guaranteed toobe of a fixed schema.</span></span>  
* <span data-ttu-id="ec284-149">Cosmos DB stöder endast strikt JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-149">Cosmos DB only supports strict JSON documents.</span></span> <span data-ttu-id="ec284-150">Det innebär hello typsystemet och uttryck är begränsad toodeal endast med JSON-typer.</span><span class="sxs-lookup"><span data-stu-id="ec284-150">This means hello type system and expressions are restricted toodeal only with JSON types.</span></span> <span data-ttu-id="ec284-151">Se toohello [JSON-specifikationen](http://www.json.org/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="ec284-151">Refer toohello [JSON specification](http://www.json.org/) for more details.</span></span>  
* <span data-ttu-id="ec284-152">En Cosmos-DB-samling är en schemafria behållare för JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-152">A Cosmos DB collection is a schema-free container of JSON documents.</span></span> <span data-ttu-id="ec284-153">hello relationer i data enheter inom och mellan dokument i en samling fångas implicit av inneslutning och inte av primärnyckel och sekundärnyckel viktiga relationer.</span><span class="sxs-lookup"><span data-stu-id="ec284-153">hello relations in data entities within and across documents in a collection are implicitly captured by containment and not by primary key and foreign key relations.</span></span> <span data-ttu-id="ec284-154">Det här är en viktig aspekt värt pekar hänsyn hello intra-dokumentet kopplingar som beskrivs senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ec284-154">This is an important aspect worth pointing out in light of hello intra-document joins discussed later in this article.</span></span>

## <span data-ttu-id="ec284-155"><a id="Indexing"></a>Cosmos DB indexering</span><span class="sxs-lookup"><span data-stu-id="ec284-155"><a id="Indexing"></a> Cosmos DB indexing</span></span>
<span data-ttu-id="ec284-156">Innan vi går in hello DocumentDB API SQL-syntax som det är värt att utforska hello indexering design i Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="ec284-156">Before we get into hello DocumentDB API SQL syntax, it is worth exploring hello indexing design in Cosmos DB.</span></span> 

<span data-ttu-id="ec284-157">hello syftar-databasindex tooserve frågorna i de olika formulär och former med minsta resursförbrukning (t.ex. CPU och in-/ utdata) och ger bra genomflöde och låg latens.</span><span class="sxs-lookup"><span data-stu-id="ec284-157">hello purpose of database indexes is tooserve queries in their various forms and shapes with minimum resource consumption (like CPU and input/output) while providing good throughput and low latency.</span></span> <span data-ttu-id="ec284-158">Hello valet av hello rätt index för att fråga en databas kräver ofta mycket planering och experiment.</span><span class="sxs-lookup"><span data-stu-id="ec284-158">Often, hello choice of hello right index for querying a database requires much planning and experimentation.</span></span> <span data-ttu-id="ec284-159">Den här metoden innebär en utmaning för schemat mindre databaser där hello data stämmer inte överens tooa strikt schema och utvecklas snabbt samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ec284-159">This approach poses a challenge for schema-less databases where hello data doesn’t conform tooa strict schema and evolves rapidly.</span></span> 

<span data-ttu-id="ec284-160">När vi hello Cosmos DB indexering undersystemet, ange vi därför hello följande mål:</span><span class="sxs-lookup"><span data-stu-id="ec284-160">Therefore, when we designed hello Cosmos DB indexing subsystem, we set hello following goals:</span></span>

* <span data-ttu-id="ec284-161">Indexera dokument utan schemat: hello indexering undersystemet inte kräver någon schemainformation eller göra några antaganden om schemat hello dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-161">Index documents without requiring schema: hello indexing subsystem does not require any schema information or make any assumptions about schema of hello documents.</span></span> 
* <span data-ttu-id="ec284-162">Stöd för effektiv och omfattande hierarkiska och relationella: hello index stöder hello Cosmos-DB-frågespråket effektivt, inklusive stöd för hierarkiska och relationella projektioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-162">Support for efficient, rich hierarchical, and relational queries: hello index supports hello Cosmos DB query language efficiently, including support for hierarchical and relational projections.</span></span>
* <span data-ttu-id="ec284-163">Stöd för konsekvent frågor in face of en varaktig volym av skrivningar: för hög skrivåtgärder genomströmning arbetsbelastningar med konsekvent frågor, hello indexet uppdateras inkrementellt effektivt och online i hello yta med en varaktig volym på skrivningar.</span><span class="sxs-lookup"><span data-stu-id="ec284-163">Support for consistent queries in face of a sustained volume of writes: For high write throughput workloads with consistent queries, hello index is updated incrementally, efficiently, and online in hello face of a sustained volume of writes.</span></span> <span data-ttu-id="ec284-164">hello konsekvent indexuppdatering är avgörande tooserve hello frågor på hello konsekvenskontroll nivå i vilken hello konfigurerade användaren hello dokumenttjänst.</span><span class="sxs-lookup"><span data-stu-id="ec284-164">hello consistent index update is crucial tooserve hello queries at hello consistency level in which hello user configured hello document service.</span></span>
* <span data-ttu-id="ec284-165">Stöd för flera innehavare: hello reservation-baserade modellen för resursen styrning angivna mellan klienter, utförs index uppdateringar hello budget systemresurser (processor, minne och i/o-åtgärder per sekund) fördelas per replik.</span><span class="sxs-lookup"><span data-stu-id="ec284-165">Support for multi-tenancy: Given hello reservation-based model for resource governance across tenants, index updates are performed within hello budget of system resources (CPU, memory, and input/output operations per second) allocated per replica.</span></span> 
* <span data-ttu-id="ec284-166">Lagringseffektivitet: för kostnadseffektivitet, hello på disken lagring omkostnader av hello index är begränsad och förutsägbara.</span><span class="sxs-lookup"><span data-stu-id="ec284-166">Storage efficiency: For cost effectiveness, hello on-disk storage overhead of hello index is bounded and predictable.</span></span> <span data-ttu-id="ec284-167">Det är viktigt eftersom Cosmos DB tillåter hello developer toomake kostnad-baserade kompromisser mellan index kostnader i relationen toohello frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="ec284-167">This is crucial because Cosmos DB allows hello developer toomake cost-based tradeoffs between index overhead in relation toohello query performance.</span></span>  

<span data-ttu-id="ec284-168">Se toohello [Azure Cosmos DB prover](https://github.com/Azure/azure-documentdb-net) på MSDN efter exempel som visar hur tooconfigure hello indexprincip för en samling.</span><span class="sxs-lookup"><span data-stu-id="ec284-168">Refer toohello [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) on MSDN for samples showing how tooconfigure hello indexing policy for a collection.</span></span> <span data-ttu-id="ec284-169">Nu är dags hello detaljer om hello Azure Cosmos-Databasens SQL-syntax.</span><span class="sxs-lookup"><span data-stu-id="ec284-169">Let’s now get into hello details of hello Azure Cosmos DB SQL syntax.</span></span>

## <span data-ttu-id="ec284-170"><a id="Basics"></a>Grunderna i Azure Cosmos-Databasens SQL-fråga</span><span class="sxs-lookup"><span data-stu-id="ec284-170"><a id="Basics"></a>Basics of an Azure Cosmos DB SQL query</span></span>
<span data-ttu-id="ec284-171">Varje fråga består av en SELECT-satsen och valfria FROM och WHERE-satser per ANSI SQL-standarder.</span><span class="sxs-lookup"><span data-stu-id="ec284-171">Every query consists of a SELECT clause and optional FROM and WHERE clauses per ANSI-SQL standards.</span></span> <span data-ttu-id="ec284-172">Vanligtvis för varje fråga räknas hello källa i FROM-satsen hello upp.</span><span class="sxs-lookup"><span data-stu-id="ec284-172">Typically, for each query, hello source in hello FROM clause is enumerated.</span></span> <span data-ttu-id="ec284-173">Sedan hello filtrera i WHERE-satsen har tillämpats på hello källa tooretrieve hello en delmängd av JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-173">Then hello filter in hello WHERE clause is applied on hello source tooretrieve a subset of JSON documents.</span></span> <span data-ttu-id="ec284-174">Slutligen hello SELECT-satsen används tooproject hello begärt JSON-värden i hello select-listan.</span><span class="sxs-lookup"><span data-stu-id="ec284-174">Finally, hello SELECT clause is used tooproject hello requested JSON values in hello select list.</span></span>

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <span data-ttu-id="ec284-175"><a id="FromClause"></a>FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="ec284-175"><a id="FromClause"></a>FROM clause</span></span>
<span data-ttu-id="ec284-176">Hej `FROM <from_specification>` satsen är valfri såvida hello källa filtreras eller planerat senare i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="ec284-176">hello `FROM <from_specification>` clause is optional unless hello source is filtered or projected later in hello query.</span></span> <span data-ttu-id="ec284-177">hello syftet med den här satsen är toospecify hello datakälla på vilka hello frågan måste fungera.</span><span class="sxs-lookup"><span data-stu-id="ec284-177">hello purpose of this clause is toospecify hello data source upon which hello query must operate.</span></span> <span data-ttu-id="ec284-178">Hello hela samlingen är ofta hello källan, men ett kan du ange en del av hello samling i stället.</span><span class="sxs-lookup"><span data-stu-id="ec284-178">Commonly hello whole collection is hello source, but one can specify a subset of hello collection instead.</span></span> 

<span data-ttu-id="ec284-179">En fråga som `SELECT * FROM Families` anger att hello hela familjer samlingen är hello källa över vilka tooenumerate.</span><span class="sxs-lookup"><span data-stu-id="ec284-179">A query like `SELECT * FROM Families` indicates that hello entire Families collection is hello source over which tooenumerate.</span></span> <span data-ttu-id="ec284-180">En särskild identifierare rot kan vara används toorepresent hello samlingen istället för att använda hello samlingens namn.</span><span class="sxs-lookup"><span data-stu-id="ec284-180">A special identifier ROOT can be used toorepresent hello collection instead of using hello collection name.</span></span> <span data-ttu-id="ec284-181">hello innehåller följande lista hello regler som tillämpas per fråga:</span><span class="sxs-lookup"><span data-stu-id="ec284-181">hello following list contains hello rules that are enforced per query:</span></span>

* <span data-ttu-id="ec284-182">hello-samling kan ha ett alias, t.ex `SELECT f.id FROM Families AS f` eller helt enkelt `SELECT f.id FROM Families f`.</span><span class="sxs-lookup"><span data-stu-id="ec284-182">hello collection can be aliased, such as `SELECT f.id FROM Families AS f` or simply `SELECT f.id FROM Families f`.</span></span> <span data-ttu-id="ec284-183">Här `f` är hello motsvarande `Families`.</span><span class="sxs-lookup"><span data-stu-id="ec284-183">Here `f` is hello equivalent of `Families`.</span></span> <span data-ttu-id="ec284-184">`AS`är en valfri nyckelordet tooalias hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="ec284-184">`AS` is an optional keyword tooalias hello identifier.</span></span>
* <span data-ttu-id="ec284-185">En gång alias hello ursprungliga källan kan inte bindas.</span><span class="sxs-lookup"><span data-stu-id="ec284-185">Once aliased, hello original source cannot be bound.</span></span> <span data-ttu-id="ec284-186">Till exempel `SELECT Families.id FROM Families f` är syntaktiskt felaktig eftersom hello identifierare ”familjer” inte kan matchas längre.</span><span class="sxs-lookup"><span data-stu-id="ec284-186">For example, `SELECT Families.id FROM Families f` is syntactically invalid since hello identifier "Families" cannot be resolved anymore.</span></span>
* <span data-ttu-id="ec284-187">Alla egenskaper som behöver toobe refererar till måste vara fullständigt kvalificerad.</span><span class="sxs-lookup"><span data-stu-id="ec284-187">All properties that need toobe referenced must be fully qualified.</span></span> <span data-ttu-id="ec284-188">Hello saknas av strikt schema, detta är tvingande tooavoid några tvetydig bindningar.</span><span class="sxs-lookup"><span data-stu-id="ec284-188">In hello absence of strict schema adherence, this is enforced tooavoid any ambiguous bindings.</span></span> <span data-ttu-id="ec284-189">Därför `SELECT id FROM Families f` är syntaktiskt felaktig sedan hello egenskapen `id` är inte bunden.</span><span class="sxs-lookup"><span data-stu-id="ec284-189">Therefore, `SELECT id FROM Families f` is syntactically invalid since hello property `id` is not bound.</span></span>

### <a name="subdocuments"></a><span data-ttu-id="ec284-190">Underdokument</span><span class="sxs-lookup"><span data-stu-id="ec284-190">Subdocuments</span></span>
<span data-ttu-id="ec284-191">hello kan också vara minskade tooa mindre deluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ec284-191">hello source can also be reduced tooa smaller subset.</span></span> <span data-ttu-id="ec284-192">Till exempel tooenumerating endast en underkatalog i varje dokument hello subroot sedan bli hello källa, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ec284-192">For instance, tooenumerating only a subtree in each document, hello subroot could then become hello source, as shown in hello following example:</span></span>

<span data-ttu-id="ec284-193">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-193">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="ec284-194">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-194">**Results**</span></span>  

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

<span data-ttu-id="ec284-195">Medan hello ovan exempel används en matris som hello källa, ett objekt även ska användas som hello källa, vilket är vad som visas i följande exempel hello: någon giltig JSON-värde (inte Odefinierad) som finns i hello källan betraktas som ska ingå i hello resultatet av hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-195">While hello above example used an array as hello source, an object could also be used as hello source, which is what's shown in hello following example: Any valid JSON value (not undefined) that can be found in hello source is considered for inclusion in hello result of hello query.</span></span> <span data-ttu-id="ec284-196">Om vissa familjer inte har en `address.state` värde, de ingår inte i hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-196">If some families don’t have an `address.state` value, they are excluded in hello query result.</span></span>

<span data-ttu-id="ec284-197">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-197">**Query**</span></span>

    SELECT * 
    FROM Families.address.state

<span data-ttu-id="ec284-198">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-198">**Results**</span></span>

    [
      "WA", 
      "NY"
    ]


## <span data-ttu-id="ec284-199"><a id="WhereClause"></a>WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="ec284-199"><a id="WhereClause"></a>WHERE clause</span></span>
<span data-ttu-id="ec284-200">Hej WHERE-satsen (**`WHERE <filter_condition>`**) är valfritt.</span><span class="sxs-lookup"><span data-stu-id="ec284-200">hello WHERE clause (**`WHERE <filter_condition>`**) is optional.</span></span> <span data-ttu-id="ec284-201">Det anger hello villkor att hello JSON-dokument som tillhandahålls av hello källa måste uppfylla i ordning toobe ingår som en del av hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-201">It specifies hello condition(s) that hello JSON documents provided by hello source must satisfy in order toobe included as part of hello result.</span></span> <span data-ttu-id="ec284-202">JSON-dokument måste utvärderas hello anges villkor för ”true” toobe för hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-202">Any JSON document must evaluate hello specified conditions too"true" toobe considered for hello result.</span></span> <span data-ttu-id="ec284-203">Hej där satsen används av hello index lager i ordning toodetermine hello absolut minsta delmängd källdokument som kan vara en del av hello resultatet.</span><span class="sxs-lookup"><span data-stu-id="ec284-203">hello WHERE clause is used by hello index layer in order toodetermine hello absolute smallest subset of source documents that can be part of hello result.</span></span> 

<span data-ttu-id="ec284-204">hello följande begäranden dokument som innehåller en namnegenskapen vars värde är `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="ec284-204">hello following query requests documents that contain a name property whose value is `AndersenFamily`.</span></span> <span data-ttu-id="ec284-205">Alla dokument som inte har en namnegenskap, eller där hello värdet matchar inte `AndersenFamily` utesluts.</span><span class="sxs-lookup"><span data-stu-id="ec284-205">Any other document that does not have a name property, or where hello value does not match `AndersenFamily` is excluded.</span></span> 

<span data-ttu-id="ec284-206">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-206">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-207">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-207">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


<span data-ttu-id="ec284-208">hello föregående exempel visade en enkel likheten fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-208">hello previous example showed a simple equality query.</span></span> <span data-ttu-id="ec284-209">DocumentDB API SQL stöder också en mängd skalära uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-209">DocumentDB API SQL also supports a variety of scalar expressions.</span></span> <span data-ttu-id="ec284-210">de vanligaste hello är binär och unära uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-210">hello most commonly used are binary and unary expressions.</span></span> <span data-ttu-id="ec284-211">Egenskapsreferenser från hello källa JSON-objekt är också giltigt uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-211">Property references from hello source JSON object are also valid expressions.</span></span> 

<span data-ttu-id="ec284-212">följande binära operatorer hello stöds för närvarande och kan användas i frågor som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ec284-212">hello following binary operators are currently supported and can be used in queries as shown in hello following examples:</span></span>  

<table>
<tr>
<td><span data-ttu-id="ec284-213">Aritmetiska</span><span class="sxs-lookup"><span data-stu-id="ec284-213">Arithmetic</span></span></td>    
<td><span data-ttu-id="ec284-214">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="ec284-214">+,-,*,/,%</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="ec284-215">Binärt</span><span class="sxs-lookup"><span data-stu-id="ec284-215">Bitwise</span></span></td>    
<td><span data-ttu-id="ec284-216">|, &, ^, <<>>,, >>> (noll fill högerskift)</span><span class="sxs-lookup"><span data-stu-id="ec284-216">|, &, ^, <<, >>, >>> (zero-fill right shift)</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="ec284-217">Logiska</span><span class="sxs-lookup"><span data-stu-id="ec284-217">Logical</span></span></td>
<td><span data-ttu-id="ec284-218">OCH, ELLER INTE</span><span class="sxs-lookup"><span data-stu-id="ec284-218">AND, OR, NOT</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="ec284-219">Jämförelse</span><span class="sxs-lookup"><span data-stu-id="ec284-219">Comparison</span></span></td>    
<td><span data-ttu-id="ec284-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span><span class="sxs-lookup"><span data-stu-id="ec284-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="ec284-221">Sträng</span><span class="sxs-lookup"><span data-stu-id="ec284-221">String</span></span></td>    
<td><span data-ttu-id="ec284-222">|| (sammanfoga)</span><span class="sxs-lookup"><span data-stu-id="ec284-222">|| (concatenate)</span></span></td>
</tr>
</table>  


<span data-ttu-id="ec284-223">Låt oss ta en titt på några frågor med binära operatorer.</span><span class="sxs-lookup"><span data-stu-id="ec284-223">Let’s take a look at some queries using binary operators.</span></span>

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


<span data-ttu-id="ec284-224">Hej unära operatorer +,-, ~ inte stöds också och kan användas inuti frågor som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ec284-224">hello unary operators +,-, ~ and NOT are also supported, and can be used inside queries as shown in hello following example:</span></span>

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



<span data-ttu-id="ec284-225">Dessutom toobinary och unära operatorer, egenskapsreferenser också tillåts.</span><span class="sxs-lookup"><span data-stu-id="ec284-225">In addition toobinary and unary operators, property references are also allowed.</span></span> <span data-ttu-id="ec284-226">Till exempel `SELECT * FROM Families f WHERE f.isRegistered` returnerar hello JSON-dokument som innehåller hello egenskapen `isRegistered` där hello egenskapens värde är lika toohello JSON `true` värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-226">For example, `SELECT * FROM Families f WHERE f.isRegistered` returns hello JSON document containing hello property `isRegistered` where hello property's value is equal toohello JSON `true` value.</span></span> <span data-ttu-id="ec284-227">Andra värden (FALSKT, null, Odefinierad, `<number>`, `<string>`, `<object>`, `<array>`osv) leder toohello källdokument som ska uteslutas från hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-227">Any other values (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`, etc.) leads toohello source document being excluded from hello result.</span></span> 

### <a name="equality-and-comparison-operators"></a><span data-ttu-id="ec284-228">Jämförelse av och likhetsfrågor operatörer</span><span class="sxs-lookup"><span data-stu-id="ec284-228">Equality and comparison operators</span></span>
<span data-ttu-id="ec284-229">hello följande tabell visar hello resultatet av likheten jämförelser i DocumentDB API SQL mellan två typer som JSON.</span><span class="sxs-lookup"><span data-stu-id="ec284-229">hello following table shows hello result of equality comparisons in DocumentDB API SQL between any two JSON types.</span></span>

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-230">
            <strong>Op</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-230">
            <strong>Op</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="ec284-231">
            <strong>Odefinierad</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-231">
            <strong>Undefined</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="ec284-232">
            <strong>Null</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-232">
            <strong>Null</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="ec284-233">
            <strong>Booleskt värde</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-233">
            <strong>Boolean</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="ec284-234">
            <strong>Antal</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-234">
            <strong>Number</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="ec284-235">
            <strong>Sträng</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-235">
            <strong>String</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="ec284-236">
            <strong>Objektet</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-236">
            <strong>Object</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="ec284-237">
            <strong>Matris</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-237">
            <strong>Array</strong>
         </span></span></td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-238">
            <strong>Odefinierad<strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-238">
            <strong>Undefined<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-239">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-239">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-240">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-240">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-241">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-241">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-242">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-242">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-243">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-243">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-244">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-244">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-245">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-245">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-246">
            <strong>Null<strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-246">
            <strong>Null<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-247">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-247">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="ec284-248">
            <strong>OKEJ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-248">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-249">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-249">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-250">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-250">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-251">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-251">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-252">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-252">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-253">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-253">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-254">
            <strong>Booleskt värde<strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-254">
            <strong>Boolean<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-255">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-255">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-256">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-256">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="ec284-257">
            <strong>OKEJ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-257">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-258">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-258">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-259">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-259">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-260">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-260">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-261">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-261">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-262">
            <strong>Antal<strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-262">
            <strong>Number<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-263">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-263">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-264">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-264">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-265">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-265">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="ec284-266">
            <strong>OKEJ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-266">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-267">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-267">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-268">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-268">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-269">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-269">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-270">
            <strong>Sträng<strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-270">
            <strong>String<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-271">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-271">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-272">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-272">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-273">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-273">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-274">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-274">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="ec284-275">
            <strong>OKEJ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-275">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-276">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-276">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-277">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-277">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-278">
            <strong>Objektet<strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-278">
            <strong>Object<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-279">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-279">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-280">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-280">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-281">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-281">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-282">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-282">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-283">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-283">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="ec284-284">
            <strong>OKEJ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-284">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-285">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-285">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="ec284-286">
            <strong>Matris<strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-286">
            <strong>Array<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="ec284-287">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-287">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-288">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-288">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-289">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-289">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-290">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-290">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-291">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-291">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="ec284-292">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-292">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="ec284-293">
            <strong>OKEJ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="ec284-293">
            <strong>OK</strong>
         </span></span></td>
      </tr>
   </tbody>
</table>

<span data-ttu-id="ec284-294">För andra jämförelseoperatorer som >, > =,! =, < och < =, hello gäller följande regler:</span><span class="sxs-lookup"><span data-stu-id="ec284-294">For other comparison operators such as >, >=, !=, < and <=, hello following rules apply:</span></span>   

* <span data-ttu-id="ec284-295">Jämförelse mellan typer resulterar i Undefined.</span><span class="sxs-lookup"><span data-stu-id="ec284-295">Comparison across types results in Undefined.</span></span>
* <span data-ttu-id="ec284-296">Jämförelse mellan två objekt eller två matriser resulterar i Undefined.</span><span class="sxs-lookup"><span data-stu-id="ec284-296">Comparison between two objects or two arrays results in Undefined.</span></span>   

<span data-ttu-id="ec284-297">Om hello resultatet av hello skalärt uttryck i hello filter är odefinierad, hello motsvarande dokumentet inte tas med i hello resultat eftersom Undefined inte logiskt del för ”true”.</span><span class="sxs-lookup"><span data-stu-id="ec284-297">If hello result of hello scalar expression in hello filter is Undefined, hello corresponding document would not be included in hello result, since Undefined doesn't logically equate too"true".</span></span>

### <a name="between-keyword"></a><span data-ttu-id="ec284-298">MELLAN nyckelord</span><span class="sxs-lookup"><span data-stu-id="ec284-298">BETWEEN keyword</span></span>
<span data-ttu-id="ec284-299">Du kan också använda hello mellan nyckelordet tooexpress frågor mot intervall med värden som i ANSI SQL.</span><span class="sxs-lookup"><span data-stu-id="ec284-299">You can also use hello BETWEEN keyword tooexpress queries against ranges of values like in ANSI SQL.</span></span> <span data-ttu-id="ec284-300">MELLAN kan användas mot strängar eller siffror.</span><span class="sxs-lookup"><span data-stu-id="ec284-300">BETWEEN can be used against strings or numbers.</span></span>

<span data-ttu-id="ec284-301">Den här frågan returnerar till exempel alla family dokument i vilka hello första underordnade klass är mellan 1-5 (båda inkluderande).</span><span class="sxs-lookup"><span data-stu-id="ec284-301">For example, this query returns all family documents in which hello first child's grade is between 1-5 (both inclusive).</span></span> 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

<span data-ttu-id="ec284-302">Till skillnad från i ANSI-SQL, kan du också använda hello BETWEEN-satsen i hello-FROM-sats som hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="ec284-302">Unlike in ANSI-SQL, you can also use hello BETWEEN clause in hello FROM clause like in hello following example.</span></span>

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

<span data-ttu-id="ec284-303">Kom ihåg toocreate en indexprincip som använder en intervallet Indextypen mot alla numeriska egenskaper/sökvägar som är filtrerade i hello BETWEEN-satsen för snabbare frågan körningstider.</span><span class="sxs-lookup"><span data-stu-id="ec284-303">For faster query execution times, remember toocreate an indexing policy that uses a range index type against any numeric properties/paths that are filtered in hello BETWEEN clause.</span></span> 

<span data-ttu-id="ec284-304">hello största skillnaden mellan att använda BETWEEN i DocumentDB-API och ANSI SQL är att du kan ange intervallet frågor mot egenskaper för olika typer – du kan till exempel ha ”klass” vara ett tal (5) i vissa dokument och strängar i andra (”grade4”).</span><span class="sxs-lookup"><span data-stu-id="ec284-304">hello main difference between using BETWEEN in DocumentDB API and ANSI SQL is that you can express range queries against properties of mixed types – for example, you might have "grade" be a number (5) in some documents and strings in others ("grade4").</span></span> <span data-ttu-id="ec284-305">I dessa fall, som i JavaScript, en jämförelse mellan två olika typer resultaten i ”Odefinierad” och hello dokumentet kommer att hoppas över.</span><span class="sxs-lookup"><span data-stu-id="ec284-305">In these cases, like in JavaScript, a comparison between two different types results in "undefined", and hello document will be skipped.</span></span>

### <a name="logical-and-or-and-not-operators"></a><span data-ttu-id="ec284-306">Logisk (AND, OR och inte) operatorer</span><span class="sxs-lookup"><span data-stu-id="ec284-306">Logical (AND, OR and NOT) operators</span></span>
<span data-ttu-id="ec284-307">Logiska operatorer fungerar med booleska värden.</span><span class="sxs-lookup"><span data-stu-id="ec284-307">Logical operators operate on Boolean values.</span></span> <span data-ttu-id="ec284-308">hello logiska sanningen tabeller för de här operatorerna visas i följande tabeller hello.</span><span class="sxs-lookup"><span data-stu-id="ec284-308">hello logical truth tables for these operators are shown in hello following tables.</span></span>

| <span data-ttu-id="ec284-309">ELLER</span><span class="sxs-lookup"><span data-stu-id="ec284-309">OR</span></span> | <span data-ttu-id="ec284-310">True</span><span class="sxs-lookup"><span data-stu-id="ec284-310">True</span></span> | <span data-ttu-id="ec284-311">False</span><span class="sxs-lookup"><span data-stu-id="ec284-311">False</span></span> | <span data-ttu-id="ec284-312">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-312">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ec284-313">True</span><span class="sxs-lookup"><span data-stu-id="ec284-313">True</span></span> |<span data-ttu-id="ec284-314">True</span><span class="sxs-lookup"><span data-stu-id="ec284-314">True</span></span> |<span data-ttu-id="ec284-315">True</span><span class="sxs-lookup"><span data-stu-id="ec284-315">True</span></span> |<span data-ttu-id="ec284-316">True</span><span class="sxs-lookup"><span data-stu-id="ec284-316">True</span></span> |
| <span data-ttu-id="ec284-317">False</span><span class="sxs-lookup"><span data-stu-id="ec284-317">False</span></span> |<span data-ttu-id="ec284-318">True</span><span class="sxs-lookup"><span data-stu-id="ec284-318">True</span></span> |<span data-ttu-id="ec284-319">False</span><span class="sxs-lookup"><span data-stu-id="ec284-319">False</span></span> |<span data-ttu-id="ec284-320">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-320">Undefined</span></span> |
| <span data-ttu-id="ec284-321">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-321">Undefined</span></span> |<span data-ttu-id="ec284-322">True</span><span class="sxs-lookup"><span data-stu-id="ec284-322">True</span></span> |<span data-ttu-id="ec284-323">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-323">Undefined</span></span> |<span data-ttu-id="ec284-324">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-324">Undefined</span></span> |

| <span data-ttu-id="ec284-325">OCH</span><span class="sxs-lookup"><span data-stu-id="ec284-325">AND</span></span> | <span data-ttu-id="ec284-326">True</span><span class="sxs-lookup"><span data-stu-id="ec284-326">True</span></span> | <span data-ttu-id="ec284-327">False</span><span class="sxs-lookup"><span data-stu-id="ec284-327">False</span></span> | <span data-ttu-id="ec284-328">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-328">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ec284-329">True</span><span class="sxs-lookup"><span data-stu-id="ec284-329">True</span></span> |<span data-ttu-id="ec284-330">True</span><span class="sxs-lookup"><span data-stu-id="ec284-330">True</span></span> |<span data-ttu-id="ec284-331">False</span><span class="sxs-lookup"><span data-stu-id="ec284-331">False</span></span> |<span data-ttu-id="ec284-332">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-332">Undefined</span></span> |
| <span data-ttu-id="ec284-333">False</span><span class="sxs-lookup"><span data-stu-id="ec284-333">False</span></span> |<span data-ttu-id="ec284-334">False</span><span class="sxs-lookup"><span data-stu-id="ec284-334">False</span></span> |<span data-ttu-id="ec284-335">False</span><span class="sxs-lookup"><span data-stu-id="ec284-335">False</span></span> |<span data-ttu-id="ec284-336">False</span><span class="sxs-lookup"><span data-stu-id="ec284-336">False</span></span> |
| <span data-ttu-id="ec284-337">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-337">Undefined</span></span> |<span data-ttu-id="ec284-338">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-338">Undefined</span></span> |<span data-ttu-id="ec284-339">False</span><span class="sxs-lookup"><span data-stu-id="ec284-339">False</span></span> |<span data-ttu-id="ec284-340">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-340">Undefined</span></span> |

| <span data-ttu-id="ec284-341">INTE</span><span class="sxs-lookup"><span data-stu-id="ec284-341">NOT</span></span> |  |
| --- | --- |
| <span data-ttu-id="ec284-342">True</span><span class="sxs-lookup"><span data-stu-id="ec284-342">True</span></span> |<span data-ttu-id="ec284-343">False</span><span class="sxs-lookup"><span data-stu-id="ec284-343">False</span></span> |
| <span data-ttu-id="ec284-344">False</span><span class="sxs-lookup"><span data-stu-id="ec284-344">False</span></span> |<span data-ttu-id="ec284-345">True</span><span class="sxs-lookup"><span data-stu-id="ec284-345">True</span></span> |
| <span data-ttu-id="ec284-346">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-346">Undefined</span></span> |<span data-ttu-id="ec284-347">Odefinierad</span><span class="sxs-lookup"><span data-stu-id="ec284-347">Undefined</span></span> |

### <a name="in-keyword"></a><span data-ttu-id="ec284-348">I nyckelord</span><span class="sxs-lookup"><span data-stu-id="ec284-348">IN keyword</span></span>
<span data-ttu-id="ec284-349">hello IN-nyckelordet kan vara används toocheck om ett angivet värde matchar något värde i en lista.</span><span class="sxs-lookup"><span data-stu-id="ec284-349">hello IN keyword can be used toocheck whether a specified value matches any value in a list.</span></span> <span data-ttu-id="ec284-350">Den här frågan returnerar till exempel alla family dokument där hello-id är ”WakefieldFamily” eller ”AndersenFamily”.</span><span class="sxs-lookup"><span data-stu-id="ec284-350">For example, this query returns all family documents where hello id is one of "WakefieldFamily" or "AndersenFamily".</span></span> 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

<span data-ttu-id="ec284-351">Det här exemplet returnerar alla dokument där hello tillstånd är någon hello angivna värden.</span><span class="sxs-lookup"><span data-stu-id="ec284-351">This example returns all documents where hello state is any of hello specified values.</span></span>

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a><span data-ttu-id="ec284-352">Ternär (?) och ansvariga för Coalesce (?)</span><span class="sxs-lookup"><span data-stu-id="ec284-352">Ternary (?) and Coalesce (??) operators</span></span>
<span data-ttu-id="ec284-353">hello Ternär och Coalesce operatörer kan vara används toobuild villkorsuttryck, liknande toopopular programmeringsspråk som C# och JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ec284-353">hello Ternary and Coalesce operators can be used toobuild conditional expressions, similar toopopular programming languages like C# and JavaScript.</span></span> 

<span data-ttu-id="ec284-354">hello Ternär (?) operator kan vara väldigt användbar när konstruera nya JSON-egenskaper på hello föra.</span><span class="sxs-lookup"><span data-stu-id="ec284-354">hello Ternary (?) operator can be very handy when constructing new JSON properties on hello fly.</span></span> <span data-ttu-id="ec284-355">Till exempel kan nu du skriva frågor tooclassify hello klassen nivåer i en mänsklig läsbar form som nybörjare/mellanliggande/Avancerat enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="ec284-355">For example, now you can write queries tooclassify hello class levels into a human readable form like Beginner/Intermediate/Advanced as shown below.</span></span>

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

<span data-ttu-id="ec284-356">Du kan också kapsla hello anrop toohello operator som i hello frågan nedan.</span><span class="sxs-lookup"><span data-stu-id="ec284-356">You can also nest hello calls toohello operator like in hello query below.</span></span>

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

<span data-ttu-id="ec284-357">Som med andra frågeoperatorer ingår om hello refererade egenskaper i hello villkorsuttrycket saknas i alla dokument, eller om hello typer som jämförs är olika, sedan dessa dokument inte i hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-357">As with other query operators, if hello referenced properties in hello conditional expression are missing in any document, or if hello types being compared are different, then those documents are excluded in hello query results.</span></span>

<span data-ttu-id="ec284-358">Hej Coalesce (?)-operatorn kan vara används tooefficiently Kontrollera (kallas även för hello förekomsten av en egenskap</span><span class="sxs-lookup"><span data-stu-id="ec284-358">hello Coalesce (??) operator can be used tooefficiently check for hello presence of a property (a.k.a.</span></span> <span data-ttu-id="ec284-359">har definierats) i ett dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-359">is defined) in a document.</span></span> <span data-ttu-id="ec284-360">Detta är användbart när frågor körs mot halvstrukturerade data av olika typer eller.</span><span class="sxs-lookup"><span data-stu-id="ec284-360">This is useful when querying against semi-structured or data of mixed types.</span></span> <span data-ttu-id="ec284-361">Den här frågan returnerar till exempel hello ”efternamn” om det finns, eller hello ”efternamn” om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="ec284-361">For example, this query returns hello "lastName" if present, or hello "surname" if it isn't present.</span></span>

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <span data-ttu-id="ec284-362"><a id="EscapingReservedKeywords"></a>Angiven egenskapsaccessor</span><span class="sxs-lookup"><span data-stu-id="ec284-362"><a id="EscapingReservedKeywords"></a>Quoted property accessor</span></span>
<span data-ttu-id="ec284-363">Du kan också komma åt egenskaper med hjälp av hello inom citattecken egenskapen operatorn `[]`.</span><span class="sxs-lookup"><span data-stu-id="ec284-363">You can also access properties using hello quoted property operator `[]`.</span></span> <span data-ttu-id="ec284-364">Till exempel `SELECT c.grade` och `SELECT c["grade"]` är likvärdiga.</span><span class="sxs-lookup"><span data-stu-id="ec284-364">For example, `SELECT c.grade` and `SELECT c["grade"]` are equivalent.</span></span> <span data-ttu-id="ec284-365">Den här syntaxen är användbart när du behöver tooescape en egenskap som innehåller blanksteg, specialtecken eller händer tooshare hello samma namn som en SQL-nyckelord eller reserverat ord.</span><span class="sxs-lookup"><span data-stu-id="ec284-365">This syntax is useful when you need tooescape a property that contains spaces, special characters, or happens tooshare hello same name as a SQL keyword or reserved word.</span></span>

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <span data-ttu-id="ec284-366"><a id="SelectClause"></a>SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="ec284-366"><a id="SelectClause"></a>SELECT clause</span></span>
<span data-ttu-id="ec284-367">hello SELECT-satsen (**`SELECT <select_list>`**) är obligatoriskt och anger vilka värden hämtas från hello fråga, precis som i ANSI SQL.</span><span class="sxs-lookup"><span data-stu-id="ec284-367">hello SELECT clause (**`SELECT <select_list>`**) is mandatory and specifies what values are retrieved from hello query, just like in ANSI-SQL.</span></span> <span data-ttu-id="ec284-368">hello delmängd som har filtrerats ovanpå hello källdokument har skickats till hello projektion fas där hello angivna JSON-värden hämtas och ett nytt JSON-objekt skapas för varje inmatningen som skickades till den.</span><span class="sxs-lookup"><span data-stu-id="ec284-368">hello subset that's been filtered on top of hello source documents are passed onto hello projection phase, where hello specified JSON values are retrieved and a new JSON object is constructed, for each input passed onto it.</span></span> 

<span data-ttu-id="ec284-369">hello som följande exempel visar en typisk urvalsfråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-369">hello following example shows a typical SELECT query.</span></span> 

<span data-ttu-id="ec284-370">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-370">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-371">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-371">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a><span data-ttu-id="ec284-372">Kapslade egenskaper</span><span class="sxs-lookup"><span data-stu-id="ec284-372">Nested properties</span></span>
<span data-ttu-id="ec284-373">I följande exempel hello, vi projicerar två kapslade egenskaper `f.address.state` och `f.address.city`.</span><span class="sxs-lookup"><span data-stu-id="ec284-373">In hello following example, we are projecting two nested properties `f.address.state` and `f.address.city`.</span></span>

<span data-ttu-id="ec284-374">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-374">**Query**</span></span>

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-375">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-375">**Results**</span></span>

    [{
      "state": "WA", 
      "city": "seattle"
    }]


<span data-ttu-id="ec284-376">Projektion stöder också JSON-uttryck som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ec284-376">Projection also supports JSON expressions as shown in hello following example:</span></span>

<span data-ttu-id="ec284-377">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-377">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-378">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-378">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


<span data-ttu-id="ec284-379">Nu ska vi titta på hello roll `$1` här.</span><span class="sxs-lookup"><span data-stu-id="ec284-379">Let's look at hello role of `$1` here.</span></span> <span data-ttu-id="ec284-380">Hej `SELECT` sats måste toocreate ett JSON-objekt och eftersom ingen nyckel tillhandahålls vi använder implicit argumentet variabelnamn som börjar med `$1`.</span><span class="sxs-lookup"><span data-stu-id="ec284-380">hello `SELECT` clause needs toocreate a JSON object and since no key is provided, we use implicit argument variable names starting with `$1`.</span></span> <span data-ttu-id="ec284-381">Den här frågan returnerar till exempel två implicit argumentvariabler, etikett `$1` och `$2`.</span><span class="sxs-lookup"><span data-stu-id="ec284-381">For example, this query returns two implicit argument variables, labeled `$1` and `$2`.</span></span>

<span data-ttu-id="ec284-382">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-382">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-383">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-383">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a><span data-ttu-id="ec284-384">Alias</span><span class="sxs-lookup"><span data-stu-id="ec284-384">Aliasing</span></span>
<span data-ttu-id="ec284-385">Nu ska vi utöka hello exemplet ovan med explicit alias med värden.</span><span class="sxs-lookup"><span data-stu-id="ec284-385">Now let's extend hello example above with explicit aliasing of values.</span></span> <span data-ttu-id="ec284-386">EFTERSOM är hello nyckelord som används för alias.</span><span class="sxs-lookup"><span data-stu-id="ec284-386">AS is hello keyword used for aliasing.</span></span> <span data-ttu-id="ec284-387">Det är valfritt som visas när projicera hello andra värdet som `NameInfo`.</span><span class="sxs-lookup"><span data-stu-id="ec284-387">It's optional as shown while projecting hello second value as `NameInfo`.</span></span> 

<span data-ttu-id="ec284-388">Om en fråga har två egenskaper med hello samma namn, alias måste vara används toorename något eller båda av hello egenskaper så att de skiljas åt i hello planerat resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-388">In case a query has two properties with hello same name, aliasing must be used toorename one or both of hello properties so that they are disambiguated in hello projected result.</span></span>

<span data-ttu-id="ec284-389">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-389">**Query**</span></span>

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-390">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-390">**Results**</span></span>

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a><span data-ttu-id="ec284-391">Skalära uttryck</span><span class="sxs-lookup"><span data-stu-id="ec284-391">Scalar expressions</span></span>
<span data-ttu-id="ec284-392">Dessutom tooproperty refererar till, hello SELECT-satsen stöder också skaläruttryck konstanter, aritmetiska uttryck, logiska uttryck osv. Här är till exempel en enkel ”Hello World”-fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-392">In addition tooproperty references, hello SELECT clause also supports scalar expressions like constants, arithmetic expressions, logical expressions, etc. For example, here's a simple "Hello World" query.</span></span>

<span data-ttu-id="ec284-393">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-393">**Query**</span></span>

    SELECT "Hello World"

<span data-ttu-id="ec284-394">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-394">**Results**</span></span>

    [{
      "$1": "Hello World"
    }]


<span data-ttu-id="ec284-395">Här är ett mer avancerat exempel som använder ett skalärt uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-395">Here's a more complex example that uses a scalar expression.</span></span>

<span data-ttu-id="ec284-396">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-396">**Query**</span></span>

    SELECT ((2 + 11 % 7)-2)/3    

<span data-ttu-id="ec284-397">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-397">**Results**</span></span>

    [{
      "$1": 1.33333
    }]


<span data-ttu-id="ec284-398">I följande exempel hello, är hello resultatet av hello skalära uttrycket ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-398">In hello following example, hello result of hello scalar expression is a Boolean.</span></span>

<span data-ttu-id="ec284-399">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-399">**Query**</span></span>

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

<span data-ttu-id="ec284-400">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-400">**Results**</span></span>

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a><span data-ttu-id="ec284-401">Skapa en objekt och matris</span><span class="sxs-lookup"><span data-stu-id="ec284-401">Object and array creation</span></span>
<span data-ttu-id="ec284-402">En annan nyckelfunktion i DocumentDB API SQL är array-objekt skapas.</span><span class="sxs-lookup"><span data-stu-id="ec284-402">Another key feature of DocumentDB API SQL is array/object creation.</span></span> <span data-ttu-id="ec284-403">Observera att vi har skapat ett nytt JSON-objekt i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="ec284-403">In hello previous example, note that we created a new JSON object.</span></span> <span data-ttu-id="ec284-404">På samma sätt kan kan en också skapa matriser som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ec284-404">Similarly, one can also construct arrays as shown in hello following examples:</span></span>

<span data-ttu-id="ec284-405">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-405">**Query**</span></span>

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

<span data-ttu-id="ec284-406">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-406">**Results**</span></span>  

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

### <span data-ttu-id="ec284-407"><a id="ValueKeyword"></a>VÄRDET nyckelord</span><span class="sxs-lookup"><span data-stu-id="ec284-407"><a id="ValueKeyword"></a>VALUE keyword</span></span>
<span data-ttu-id="ec284-408">Hej **värdet** nyckelordet ger ett sätt tooreturn JSON-värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-408">hello **VALUE** keyword provides a way tooreturn JSON value.</span></span> <span data-ttu-id="ec284-409">Till exempel hello frågan nedan returnerar hello skalära `"Hello World"` i stället för `{$1: "Hello World"}`.</span><span class="sxs-lookup"><span data-stu-id="ec284-409">For example, hello query shown below returns hello scalar `"Hello World"` instead of `{$1: "Hello World"}`.</span></span>

<span data-ttu-id="ec284-410">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-410">**Query**</span></span>

    SELECT VALUE "Hello World"

<span data-ttu-id="ec284-411">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-411">**Results**</span></span>

    [
      "Hello World"
    ]


<span data-ttu-id="ec284-412">hello följande fråga returnerar hello JSON-värde utan hello `"address"` etikett i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-412">hello following query returns hello JSON value without hello `"address"` label in hello results.</span></span>

<span data-ttu-id="ec284-413">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-413">**Query**</span></span>

    SELECT VALUE f.address
    FROM Families f    

<span data-ttu-id="ec284-414">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-414">**Results**</span></span>  

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

<span data-ttu-id="ec284-415">hello följande exempel utökar den här tooshow hur tooreturn JSON primitiva värden (hello lövnivån av hello JSON-träd).</span><span class="sxs-lookup"><span data-stu-id="ec284-415">hello following example extends this tooshow how tooreturn JSON primitive values (hello leaf level of hello JSON tree).</span></span> 

<span data-ttu-id="ec284-416">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-416">**Query**</span></span>

    SELECT VALUE f.address.state
    FROM Families f    

<span data-ttu-id="ec284-417">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-417">**Results**</span></span>

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a><span data-ttu-id="ec284-418">* Operatorn</span><span class="sxs-lookup"><span data-stu-id="ec284-418">* Operator</span></span>
<span data-ttu-id="ec284-419">hello särskilda operatorn (*) är stöds tooproject hello dokumentet-är.</span><span class="sxs-lookup"><span data-stu-id="ec284-419">hello special operator (*) is supported tooproject hello document as-is.</span></span> <span data-ttu-id="ec284-420">När den används, måste det vara hello endast beräknade fält.</span><span class="sxs-lookup"><span data-stu-id="ec284-420">When used, it must be hello only projected field.</span></span> <span data-ttu-id="ec284-421">När en fråga som `SELECT * FROM Families f` är giltigt, `SELECT VALUE * FROM Families f ` och `SELECT *, f.id FROM Families f ` är inte giltiga.</span><span class="sxs-lookup"><span data-stu-id="ec284-421">While a query like `SELECT * FROM Families f` is valid, `SELECT VALUE * FROM Families f ` and  `SELECT *, f.id FROM Families f ` are not valid.</span></span>

<span data-ttu-id="ec284-422">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-422">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="ec284-423">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-423">**Results**</span></span>

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

### <span data-ttu-id="ec284-424"><a id="TopKeyword"></a>Operatorn TOP</span><span class="sxs-lookup"><span data-stu-id="ec284-424"><a id="TopKeyword"></a>TOP Operator</span></span>
<span data-ttu-id="ec284-425">hello ÖVERSTA nyckelord kan vara används toolimit hello antal värden från en fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-425">hello TOP keyword can be used toolimit hello number of values from a query.</span></span> <span data-ttu-id="ec284-426">När upp används tillsammans med hello ORDER BY-sats, är hello resultatuppsättningen begränsad toohello N första beställda värden. Annars returneras hello N första resultat i en odefinierad ordning.</span><span class="sxs-lookup"><span data-stu-id="ec284-426">When TOP is used in conjunction with hello ORDER BY clause, hello result set is limited toohello first N number of ordered values; otherwise, it returns hello first N number of results in an undefined order.</span></span> <span data-ttu-id="ec284-427">Som bästa praxis, i en SELECT-instruktion alltid Använd en ORDER BY-sats med hello TOP-instruktion.</span><span class="sxs-lookup"><span data-stu-id="ec284-427">As a best practice, in a SELECT statement, always use an ORDER BY clause with hello TOP clause.</span></span> <span data-ttu-id="ec284-428">Detta är hello endast sätt toopredictably anger vilka rader som påverkas av ÖVERKANT.</span><span class="sxs-lookup"><span data-stu-id="ec284-428">This is hello only way toopredictably indicate which rows are affected by TOP.</span></span> 

<span data-ttu-id="ec284-429">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-429">**Query**</span></span>

    SELECT TOP 1 * 
    FROM Families f 

<span data-ttu-id="ec284-430">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-430">**Results**</span></span>

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

<span data-ttu-id="ec284-431">TOP kan användas med ett konstant värde (som visas ovan) eller med ett variabelvärde med hjälp av frågor med parametrar.</span><span class="sxs-lookup"><span data-stu-id="ec284-431">TOP can be used with a constant value (as shown above) or with a variable value using parameterized queries.</span></span> <span data-ttu-id="ec284-432">Mer information finns i parameterfrågor nedan.</span><span class="sxs-lookup"><span data-stu-id="ec284-432">For more details, please see parameterized queries below.</span></span>

### <span data-ttu-id="ec284-433"><a id="Aggregates"></a>Mängdfunktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-433"><a id="Aggregates"></a>Aggregate Functions</span></span>
<span data-ttu-id="ec284-434">Du kan också utföra aggregeringar i hello `SELECT` satsen.</span><span class="sxs-lookup"><span data-stu-id="ec284-434">You can also perform aggregations in hello `SELECT` clause.</span></span> <span data-ttu-id="ec284-435">Mängdfunktioner utför en beräkning på en uppsättning värden och returnera ett enstaka värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-435">Aggregate functions perform a calculation on a set of values and return a single value.</span></span> <span data-ttu-id="ec284-436">Till exempel returnerar hello följande fråga hello antal family dokument inom hello samlingen.</span><span class="sxs-lookup"><span data-stu-id="ec284-436">For example, hello following query returns hello count of family documents within hello collection.</span></span>

<span data-ttu-id="ec284-437">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-437">**Query**</span></span>

    SELECT COUNT(1) 
    FROM Families f 

<span data-ttu-id="ec284-438">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-438">**Results**</span></span>

    [{
        "$1": 2
    }]

<span data-ttu-id="ec284-439">Du kan också returnera hello skalära värde hello sammanställd med hjälp av hello `VALUE` nyckelord.</span><span class="sxs-lookup"><span data-stu-id="ec284-439">You can also return hello scalar value of hello aggregate by using hello `VALUE` keyword.</span></span> <span data-ttu-id="ec284-440">Till exempel returnerar hello följande fråga hello antalet värden som ett tal:</span><span class="sxs-lookup"><span data-stu-id="ec284-440">For example, hello following query returns hello count of values as a single number:</span></span>

<span data-ttu-id="ec284-441">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-441">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f 

<span data-ttu-id="ec284-442">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-442">**Results**</span></span>

    [ 2 ]

<span data-ttu-id="ec284-443">Du kan också utföra aggregeringar i kombination med filter.</span><span class="sxs-lookup"><span data-stu-id="ec284-443">You can also perform aggregates in combination with filters.</span></span> <span data-ttu-id="ec284-444">Till exempel returnerar hello följande fråga hello antal dokument med hello-adress i hello delstaten Washington.</span><span class="sxs-lookup"><span data-stu-id="ec284-444">For example, hello following query returns hello count of documents with hello address in hello state of Washington.</span></span>

<span data-ttu-id="ec284-445">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-445">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

<span data-ttu-id="ec284-446">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-446">**Results**</span></span>

    [ 1 ]

<span data-ttu-id="ec284-447">hello listas följande tabell hello stöds mängdfunktioner i DocumentDB-API.</span><span class="sxs-lookup"><span data-stu-id="ec284-447">hello following table shows hello list of supported aggregate functions in DocumentDB API.</span></span> <span data-ttu-id="ec284-448">`SUM`och `AVG` utförs via numeriska värden, medan `COUNT`, `MIN`, och `MAX` kan utföras via siffror, strängar, booleska värden och null-värden.</span><span class="sxs-lookup"><span data-stu-id="ec284-448">`SUM` and `AVG` are performed over numeric values, whereas `COUNT`, `MIN`, and `MAX` can be performed over numbers, strings, Booleans, and nulls.</span></span> 

| <span data-ttu-id="ec284-449">Användning</span><span class="sxs-lookup"><span data-stu-id="ec284-449">Usage</span></span> | <span data-ttu-id="ec284-450">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec284-450">Description</span></span> |
|-------|-------------|
| <span data-ttu-id="ec284-451">ANTAL</span><span class="sxs-lookup"><span data-stu-id="ec284-451">COUNT</span></span> | <span data-ttu-id="ec284-452">Returnerar hello antal objekt i hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-452">Returns hello number of items in hello expression.</span></span> |
| <span data-ttu-id="ec284-453">SUMMA</span><span class="sxs-lookup"><span data-stu-id="ec284-453">SUM</span></span>   | <span data-ttu-id="ec284-454">Returnerar hello summan av alla hello-värden i hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-454">Returns hello sum of all hello values in hello expression.</span></span> |
| <span data-ttu-id="ec284-455">MIN</span><span class="sxs-lookup"><span data-stu-id="ec284-455">MIN</span></span>   | <span data-ttu-id="ec284-456">Returnerar hello minimivärdet i hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-456">Returns hello minimum value in hello expression.</span></span> |
| <span data-ttu-id="ec284-457">MAX</span><span class="sxs-lookup"><span data-stu-id="ec284-457">MAX</span></span>   | <span data-ttu-id="ec284-458">Returnerar hello maxvärdet i hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-458">Returns hello maximum value in hello expression.</span></span> |
| <span data-ttu-id="ec284-459">GENOMSN.</span><span class="sxs-lookup"><span data-stu-id="ec284-459">AVG</span></span>   | <span data-ttu-id="ec284-460">Returnerar hello medelvärdet av hello i hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-460">Returns hello average of hello values in hello expression.</span></span> |

<span data-ttu-id="ec284-461">Aggregeringar kan också utföras via hello resultaten av en matris iteration.</span><span class="sxs-lookup"><span data-stu-id="ec284-461">Aggregates can also be performed over hello results of an array iteration.</span></span> <span data-ttu-id="ec284-462">Mer information finns i [matris Iteration i frågor](#Iteration).</span><span class="sxs-lookup"><span data-stu-id="ec284-462">For more information, see [Array Iteration in Queries](#Iteration).</span></span>

> [!NOTE]
> <span data-ttu-id="ec284-463">När du använder hello Azure portal Frågeutforskaren, Observera att aggregering frågor kan returnera hello delvis sammanlagda resultat via en fråga sida.</span><span class="sxs-lookup"><span data-stu-id="ec284-463">When using hello Azure portal's Query Explorer, note that aggregation queries may return hello partially aggregated results over a query page.</span></span> <span data-ttu-id="ec284-464">hello SDK producerar en enda ackumulerade värdet på alla sidor.</span><span class="sxs-lookup"><span data-stu-id="ec284-464">hello SDKs produces a single cumulative value across all pages.</span></span> 
> 
> <span data-ttu-id="ec284-465">I ordning tooperform aggregering frågor med kod, som du behöver .NET SDK 1.12.0, .NET Core SDK 1.1.0 eller Java SDK 1.9.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ec284-465">In order tooperform aggregation queries using code, you need .NET SDK 1.12.0, .NET Core SDK 1.1.0, or Java SDK 1.9.5 or above.</span></span>    
>

## <span data-ttu-id="ec284-466"><a id="OrderByClause"></a>ORDER BY-sats</span><span class="sxs-lookup"><span data-stu-id="ec284-466"><a id="OrderByClause"></a>ORDER BY clause</span></span>
<span data-ttu-id="ec284-467">Som i ANSI-SQL, kan du använda en valfri Order By-sats samtidigt som frågor körs.</span><span class="sxs-lookup"><span data-stu-id="ec284-467">Like in ANSI-SQL, you can include an optional Order By clause while querying.</span></span> <span data-ttu-id="ec284-468">hello-sats kan innehålla en valfri ASC/DESC argumentet toospecify hello order som resultat måste hämtas.</span><span class="sxs-lookup"><span data-stu-id="ec284-468">hello clause can include an optional ASC/DESC argument toospecify hello order in which results must be retrieved.</span></span>

<span data-ttu-id="ec284-469">Här är till exempel en fråga som hämtar familjer i den ordning de hello resident stad namn.</span><span class="sxs-lookup"><span data-stu-id="ec284-469">For example, here's a query that retrieves families in order of hello resident city's name.</span></span>

<span data-ttu-id="ec284-470">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-470">**Query**</span></span>

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

<span data-ttu-id="ec284-471">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-471">**Results**</span></span>

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

<span data-ttu-id="ec284-472">Här är en fråga som hämtar familjer efter skapandedatum som lagras som ett tal som representerar hello epok tid, dvs, förfluten tid sedan den 1 januari 1970 i sekunder.</span><span class="sxs-lookup"><span data-stu-id="ec284-472">And here's a query that retrieves families in order of creation date, which is stored as a number representing hello epoch time, i.e, elapsed time since Jan 1, 1970 in seconds.</span></span>

<span data-ttu-id="ec284-473">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-473">**Query**</span></span>

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

<span data-ttu-id="ec284-474">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-474">**Results**</span></span>

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

## <span data-ttu-id="ec284-475"><a id="Advanced"></a>Avancerade begrepp och SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="ec284-475"><a id="Advanced"></a>Advanced database concepts and SQL queries</span></span>

### <span data-ttu-id="ec284-476"><a id="Iteration"></a>Upprepning</span><span class="sxs-lookup"><span data-stu-id="ec284-476"><a id="Iteration"></a>Iteration</span></span>
<span data-ttu-id="ec284-477">En ny konstruktion har lagts till via hello **IN** nyckelord i DocumentDB API SQL tooprovide stöd för att iterera över JSON-matriser.</span><span class="sxs-lookup"><span data-stu-id="ec284-477">A new construct was added via hello **IN** keyword in DocumentDB API SQL tooprovide support for iterating over JSON arrays.</span></span> <span data-ttu-id="ec284-478">Hej från datakällan har stöd för iteration.</span><span class="sxs-lookup"><span data-stu-id="ec284-478">hello FROM source provides support for iteration.</span></span> <span data-ttu-id="ec284-479">Vi börjar med hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="ec284-479">Let's start with hello following example:</span></span>

<span data-ttu-id="ec284-480">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-480">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="ec284-481">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-481">**Results**</span></span>  

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

<span data-ttu-id="ec284-482">Nu ska vi titta på en annan fråga som utför iteration över underordnade i hello samling.</span><span class="sxs-lookup"><span data-stu-id="ec284-482">Now let's look at another query that performs iteration over children in hello collection.</span></span> <span data-ttu-id="ec284-483">Observera hello skillnaden i hello utdata matris.</span><span class="sxs-lookup"><span data-stu-id="ec284-483">Note hello difference in hello output array.</span></span> <span data-ttu-id="ec284-484">Det här exemplet delar upp `children` och förenklas hello resultat i en matris.</span><span class="sxs-lookup"><span data-stu-id="ec284-484">This example splits `children` and flattens hello results into a single array.</span></span>  

<span data-ttu-id="ec284-485">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-485">**Query**</span></span>

    SELECT * 
    FROM c IN Families.children

<span data-ttu-id="ec284-486">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-486">**Results**</span></span>  

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

<span data-ttu-id="ec284-487">Detta kan vara mer används toofilter på varje enskild post hello matrisen som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ec284-487">This can be further used toofilter on each individual entry of hello array as shown in hello following example:</span></span>

<span data-ttu-id="ec284-488">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-488">**Query**</span></span>

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

<span data-ttu-id="ec284-489">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-489">**Results**</span></span>  

    [{
      "givenName": "Lisa"
    }]

<span data-ttu-id="ec284-490">Du kan också utföra sammanställning över hello resultatet av matrisen iteration.</span><span class="sxs-lookup"><span data-stu-id="ec284-490">You can also perform aggregation over hello result of array iteration.</span></span> <span data-ttu-id="ec284-491">Till exempel räknar hello följande fråga hello antalet underordnade objekt mellan alla serier.</span><span class="sxs-lookup"><span data-stu-id="ec284-491">For example, hello following query counts hello number of children among all families.</span></span>

<span data-ttu-id="ec284-492">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-492">**Query**</span></span>

    SELECT COUNT(child) 
    FROM child IN Families.children

<span data-ttu-id="ec284-493">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-493">**Results**</span></span>  

    [
      { 
        "$1": 3
      }
    ]

### <span data-ttu-id="ec284-494"><a id="Joins"></a>Kopplingar</span><span class="sxs-lookup"><span data-stu-id="ec284-494"><a id="Joins"></a>Joins</span></span>
<span data-ttu-id="ec284-495">Hello måste toojoin över tabeller är viktigt i en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="ec284-495">In a relational database, hello need toojoin across tables is important.</span></span> <span data-ttu-id="ec284-496">Dess hello logiska naturlig följd toodesigning normaliserade scheman.</span><span class="sxs-lookup"><span data-stu-id="ec284-496">It's hello logical corollary toodesigning normalized schemas.</span></span> <span data-ttu-id="ec284-497">Motstridiga toothis, DocumentDB API behandlar hello Avnormaliserade datamodellen schemafria dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-497">Contrary toothis, DocumentDB API deals with hello denormalized data model of schema-free documents.</span></span> <span data-ttu-id="ec284-498">Detta är hello logiska motsvarigheten till en ”självkoppling”.</span><span class="sxs-lookup"><span data-stu-id="ec284-498">This is hello logical equivalent of a "self-join".</span></span>

<span data-ttu-id="ec284-499">hello-syntax som hello språket stöder är < from_source1 > < from_source2 > Anslut till koppling... Anslut < from_sourceN >.</span><span class="sxs-lookup"><span data-stu-id="ec284-499">hello syntax that hello language supports is <from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>.</span></span> <span data-ttu-id="ec284-500">Generellt sett detta returnerar en uppsättning **N**- tupplar (tuppel med **N** värden).</span><span class="sxs-lookup"><span data-stu-id="ec284-500">Overall, this returns a set of **N**-tuples (tuple with **N** values).</span></span> <span data-ttu-id="ec284-501">Varje tuppel har värden som genereras av alla samling alias iterera över sina respektive uppsättningar.</span><span class="sxs-lookup"><span data-stu-id="ec284-501">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span> <span data-ttu-id="ec284-502">Detta är med andra ord en fullständig kryssprodukten av hello mängder som deltar i hello koppling.</span><span class="sxs-lookup"><span data-stu-id="ec284-502">In other words, this is a full cross product of hello sets participating in hello join.</span></span>

<span data-ttu-id="ec284-503">hello följande exempel visar hur det fungerar hello JOIN-sats.</span><span class="sxs-lookup"><span data-stu-id="ec284-503">hello following examples show how hello JOIN clause works.</span></span> <span data-ttu-id="ec284-504">I följande exempel hello, kan hello resultatet är tom eftersom hello kryssprodukten av varje dokument från käll- och tomma är tom.</span><span class="sxs-lookup"><span data-stu-id="ec284-504">In hello following example, hello result is empty since hello cross product of each document from source and an empty set is empty.</span></span>

<span data-ttu-id="ec284-505">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-505">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

<span data-ttu-id="ec284-506">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-506">**Results**</span></span>  

    [{
    }]


<span data-ttu-id="ec284-507">I följande exempel hello, är hello koppling mellan hello dokumentroten och hello `children` subroot.</span><span class="sxs-lookup"><span data-stu-id="ec284-507">In hello following example, hello join is between hello document root and hello `children` subroot.</span></span> <span data-ttu-id="ec284-508">Det är en kryssprodukten mellan två JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="ec284-508">It's a cross product between two JSON objects.</span></span> <span data-ttu-id="ec284-509">hello faktum att underordnade är en matris är inte effektivt i hello koppling eftersom vi arbetar med en enda rot är hello underordnade matris.</span><span class="sxs-lookup"><span data-stu-id="ec284-509">hello fact that children is an array is not effective in hello JOIN since we are dealing with a single root that is hello children array.</span></span> <span data-ttu-id="ec284-510">Därför innehåller hello resultatet bara två resultat eftersom hello kryssprodukten av varje dokument med hello matris ger exakt bara ett dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-510">Hence hello result contains only two results, since hello cross product of each document with hello array yields exactly only one document.</span></span>

<span data-ttu-id="ec284-511">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-511">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.children

<span data-ttu-id="ec284-512">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-512">**Results**</span></span>

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


<span data-ttu-id="ec284-513">hello som följande exempel visar en mer konventionella koppling:</span><span class="sxs-lookup"><span data-stu-id="ec284-513">hello following example shows a more conventional join:</span></span>

<span data-ttu-id="ec284-514">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-514">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

<span data-ttu-id="ec284-515">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-515">**Results**</span></span>

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



<span data-ttu-id="ec284-516">hello först öppna toonote är att hello `from_source` av hello **ansluta** -satsen är en iterator.</span><span class="sxs-lookup"><span data-stu-id="ec284-516">hello first thing toonote is that hello `from_source` of hello **JOIN** clause is an iterator.</span></span> <span data-ttu-id="ec284-517">Därför hello flödet är i det här fallet följande:</span><span class="sxs-lookup"><span data-stu-id="ec284-517">So, hello flow in this case is as follows:</span></span>  

* <span data-ttu-id="ec284-518">Expandera varje element i underordnade **c** i hello matris.</span><span class="sxs-lookup"><span data-stu-id="ec284-518">Expand each child element **c** in hello array.</span></span>
* <span data-ttu-id="ec284-519">Tillämpa en kryssprodukten med hello hello dokumentets rot **f** med varje underordnat element **c** som har förenklad i hello första steget.</span><span class="sxs-lookup"><span data-stu-id="ec284-519">Apply a cross product with hello root of hello document **f** with each child element **c** that was flattened in hello first step.</span></span>
* <span data-ttu-id="ec284-520">Slutligen projektet hello rotobjektet **f** namnegenskap enbart.</span><span class="sxs-lookup"><span data-stu-id="ec284-520">Finally, project hello root object **f** name property alone.</span></span> 

<span data-ttu-id="ec284-521">hello första dokumentet (`AndersenFamily`) innehåller endast ett underordnat element, så hello resultatet innehåller bara ett enda objekt motsvarande toothis dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-521">hello first document (`AndersenFamily`) contains only one child element, so hello result set contains only a single object corresponding toothis document.</span></span> <span data-ttu-id="ec284-522">hello andra dokumentet (`WakefieldFamily`) innehåller två underordnade.</span><span class="sxs-lookup"><span data-stu-id="ec284-522">hello second document (`WakefieldFamily`) contains two children.</span></span> <span data-ttu-id="ec284-523">Därför genererar hello kryssprodukten ett separat objekt för varje underordnad, vilket ledde till två objekt, en för varje underordnad motsvarande toothis dokument.</span><span class="sxs-lookup"><span data-stu-id="ec284-523">So, hello cross product produces a separate object for each child, thereby resulting in two objects, one for each child corresponding toothis document.</span></span> <span data-ttu-id="ec284-524">hello rot fält i båda dessa dokument är hello detsamma, precis som du förväntar dig i en kryssprodukten.</span><span class="sxs-lookup"><span data-stu-id="ec284-524">hello root fields in both these documents are hello same, just as you would expect in a cross product.</span></span>

<span data-ttu-id="ec284-525">hello riktigt utility av hello koppling är tooform tupplar från hello-kryssprodukten i en form som annars svårt tooproject.</span><span class="sxs-lookup"><span data-stu-id="ec284-525">hello real utility of hello JOIN is tooform tuples from hello cross-product in a shape that's otherwise difficult tooproject.</span></span> <span data-ttu-id="ec284-526">Som vi se i hello exemplet nedan kan du dessutom kunde filtrera på hello kombination av en tuppel som kan hello användaren har valt ett villkor uppfylls av hello tupplar övergripande.</span><span class="sxs-lookup"><span data-stu-id="ec284-526">Furthermore, as we see in hello example below, you could filter on hello combination of a tuple that lets' hello user chose a condition satisfied by hello tuples overall.</span></span>

<span data-ttu-id="ec284-527">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-527">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

<span data-ttu-id="ec284-528">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-528">**Results**</span></span>

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



<span data-ttu-id="ec284-529">Det här exemplet är en naturlig förlängning av hello föregående exempel och utför en dubbel koppling.</span><span class="sxs-lookup"><span data-stu-id="ec284-529">This example is a natural extension of hello preceding example, and performs a double join.</span></span> <span data-ttu-id="ec284-530">Därför kan hello kryssprodukten visas som hello följande genererat kod:</span><span class="sxs-lookup"><span data-stu-id="ec284-530">So, hello cross product can be viewed as hello following pseudo-code:</span></span>

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

<span data-ttu-id="ec284-531">`AndersenFamily`har ett underordnat objekt som har en husdjur.</span><span class="sxs-lookup"><span data-stu-id="ec284-531">`AndersenFamily` has one child who has one pet.</span></span> <span data-ttu-id="ec284-532">Därför hello kryssprodukten ger en rad (1\*1\*1) från den här serien.</span><span class="sxs-lookup"><span data-stu-id="ec284-532">So, hello cross product yields one row (1\*1\*1) from this family.</span></span> <span data-ttu-id="ec284-533">WakefieldFamily men har två underordnade objekt, men endast ett underordnat objekt ”Jesse” har husdjur.</span><span class="sxs-lookup"><span data-stu-id="ec284-533">WakefieldFamily however has two children, but only one child "Jesse" has pets.</span></span> <span data-ttu-id="ec284-534">Jesse har två husdjur om.</span><span class="sxs-lookup"><span data-stu-id="ec284-534">Jesse has two pets though.</span></span> <span data-ttu-id="ec284-535">Därför hello kryssprodukten ger 1\*1\*2 = 2 rader från den här serien.</span><span class="sxs-lookup"><span data-stu-id="ec284-535">Hence hello cross product yields 1\*1\*2 = 2 rows from this family.</span></span>

<span data-ttu-id="ec284-536">I nästa exempel hello finns ett filter på `pet`.</span><span class="sxs-lookup"><span data-stu-id="ec284-536">In hello next example, there is an additional filter on `pet`.</span></span> <span data-ttu-id="ec284-537">Detta omfattar inte alla hello tupplar där hello husdjur namn inte är ”skuggkopia”.</span><span class="sxs-lookup"><span data-stu-id="ec284-537">This excludes all hello tuples where hello pet name is not "Shadow".</span></span> <span data-ttu-id="ec284-538">Observera att vi kan toobuild tupplar från matriser, filter på hello-element för hello tuppel och projektet valfri kombination av hello-element.</span><span class="sxs-lookup"><span data-stu-id="ec284-538">Notice that we are able toobuild tuples from arrays, filter on any of hello elements of hello tuple, and project any combination of hello elements.</span></span> 

<span data-ttu-id="ec284-539">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-539">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

<span data-ttu-id="ec284-540">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-540">**Results**</span></span>

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <span data-ttu-id="ec284-541"><a id="JavaScriptIntegration"></a>JavaScript-integrering</span><span class="sxs-lookup"><span data-stu-id="ec284-541"><a id="JavaScriptIntegration"></a>JavaScript integration</span></span>
<span data-ttu-id="ec284-542">Azure Cosmos-DB är en programmeringsmodell för att köra JavaScript-baserade programlogik direkt på hello samlingar som lagrade procedurer och utlösare.</span><span class="sxs-lookup"><span data-stu-id="ec284-542">Azure Cosmos DB provides a programming model for executing JavaScript based application logic directly on hello collections in terms of stored procedures and triggers.</span></span> <span data-ttu-id="ec284-543">Detta möjliggör både:</span><span class="sxs-lookup"><span data-stu-id="ec284-543">This allows for both:</span></span>

* <span data-ttu-id="ec284-544">Möjlighet-toodo högpresterande transaktionella CRUD-åtgärder och frågor mot dokument i en samling tack vare hello djupgående integration av JavaScript-körning direkt i databasmotorn hello.</span><span class="sxs-lookup"><span data-stu-id="ec284-544">Ability toodo high-performance transactional CRUD operations and queries against documents in a collection by virtue of hello deep integration of JavaScript runtime directly within hello database engine.</span></span> 
* <span data-ttu-id="ec284-545">En fysisk modellering av Kontrollflöde, variabel omfång och tilldelning och integration av undantagshantering primitiver med databastransaktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-545">A natural modeling of control flow, variable scoping, and assignment and integration of exception handling primitives with database transactions.</span></span> <span data-ttu-id="ec284-546">Mer information om Azure DB som Cosmos-stöd för JavaScript-integrering finns toohello JavaScript programmering av serversidan dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ec284-546">For more details about Azure Cosmos DB support for JavaScript integration, please refer toohello JavaScript server-side programmability documentation.</span></span>

### <span data-ttu-id="ec284-547"><a id="UserDefinedFunctions"></a>Användardefinierade funktioner (UDF)</span><span class="sxs-lookup"><span data-stu-id="ec284-547"><a id="UserDefinedFunctions"></a>User-Defined Functions (UDFs)</span></span>
<span data-ttu-id="ec284-548">DocumentDB API SQL ger stöd för användaren definierat funktioner (UDF) tillsammans med hello-typer som har redan definierats i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ec284-548">Along with hello types already defined in this article, DocumentDB API SQL provides support for User Defined Functions (UDF).</span></span> <span data-ttu-id="ec284-549">I synnerhet stöds skalära UDF: er där hello utvecklare kan skicka in noll eller flera argument och returnera ett enda argument resultat tillbaka.</span><span class="sxs-lookup"><span data-stu-id="ec284-549">In particular, scalar UDFs are supported where hello developers can pass in zero or many arguments and return a single argument result back.</span></span> <span data-ttu-id="ec284-550">Var och en av de här argumenten kontrolleras för att vara giltiga JSON-värdena.</span><span class="sxs-lookup"><span data-stu-id="ec284-550">Each of these arguments is checked for being legal JSON values.</span></span>  

<span data-ttu-id="ec284-551">Hej DocumentDB API SQL-syntax är utökat toosupport anpassad programlogik med hjälp av dessa användardefinierade funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-551">hello DocumentDB API SQL syntax is extended toosupport custom application logic using these User-Defined Functions.</span></span> <span data-ttu-id="ec284-552">UDF: er kan registreras med DocumentDB-API och sedan refereras som en del av en SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-552">UDFs can be registered with DocumentDB API and then be referenced as part of a SQL query.</span></span> <span data-ttu-id="ec284-553">Faktum är utformad hello UDF: er är exquisitely toobe som anropas av frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-553">In fact, hello UDFs are exquisitely designed toobe invoked by queries.</span></span> <span data-ttu-id="ec284-554">Som en naturlig följd toothis alternativ UDF: er har inte åtkomst toohello context-objektet som hello andra JavaScript typer (lagrade procedurer och utlösare) har.</span><span class="sxs-lookup"><span data-stu-id="ec284-554">As a corollary toothis choice, UDFs do not have access toohello context object which hello other JavaScript types (stored procedures and triggers) have.</span></span> <span data-ttu-id="ec284-555">Eftersom frågor körs i skrivskyddat läge kan köras de på primära eller sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="ec284-555">Since queries execute as read-only, they can run either on primary or on secondary replicas.</span></span> <span data-ttu-id="ec284-556">Därför är UDF: er utformad toorun på sekundära repliker till skillnad från andra typer av JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ec284-556">Therefore, UDFs are designed toorun on secondary replicas unlike other JavaScript types.</span></span>

<span data-ttu-id="ec284-557">Nedan visas ett exempel på hur en UDF kan registreras på hello Cosmos-DB-databas, speciellt under en dokumentsamling.</span><span class="sxs-lookup"><span data-stu-id="ec284-557">Below is an example of how a UDF can be registered at hello Cosmos DB database, specifically under a document collection.</span></span>

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

<span data-ttu-id="ec284-558">hello exemplet ovan skapar en UDF-fil som heter `REGEX_MATCH`.</span><span class="sxs-lookup"><span data-stu-id="ec284-558">hello preceding example creates a UDF whose name is `REGEX_MATCH`.</span></span> <span data-ttu-id="ec284-559">Den accepterar två strängvärden som JSON `input` och `pattern` och kontrollerar om hello första matchar hello mönster som anges i andra hello använder Javascript's string.match()-funktionen.</span><span class="sxs-lookup"><span data-stu-id="ec284-559">It accepts two JSON string values `input` and `pattern` and checks if hello first matches hello pattern specified in hello second using JavaScript's string.match() function.</span></span>

<span data-ttu-id="ec284-560">Vi kan nu använda den här UDF i en fråga i en projektion.</span><span class="sxs-lookup"><span data-stu-id="ec284-560">We can now use this UDF in a query in a projection.</span></span> <span data-ttu-id="ec284-561">UDF: er måste vara kvalificerat med hello skiftlägeskänsliga prefixet ”udf”.</span><span class="sxs-lookup"><span data-stu-id="ec284-561">UDFs must be qualified with hello case-sensitive prefix "udf."</span></span> <span data-ttu-id="ec284-562">När anropas från frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-562">when called from within queries.</span></span> 

> [!NOTE]
> <span data-ttu-id="ec284-563">Tidigare too3/17/2015, Cosmos DB stöds UDF anrop utan hello ”udf”.</span><span class="sxs-lookup"><span data-stu-id="ec284-563">Prior too3/17/2015, Cosmos DB supported UDF calls without hello "udf."</span></span> <span data-ttu-id="ec284-564">prefix som väljer REGEX_MATCH().</span><span class="sxs-lookup"><span data-stu-id="ec284-564">prefix like SELECT REGEX_MATCH().</span></span> <span data-ttu-id="ec284-565">Det här anropa mönstret är föråldrad.</span><span class="sxs-lookup"><span data-stu-id="ec284-565">This calling pattern has been deprecated.</span></span>  
> 
> 

<span data-ttu-id="ec284-566">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-566">**Query**</span></span>

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

<span data-ttu-id="ec284-567">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-567">**Results**</span></span>

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

<span data-ttu-id="ec284-568">hello UDF kan också användas i ett filter som visas i hello exemplet nedan är också kvalificerad med hello ”udf”.</span><span class="sxs-lookup"><span data-stu-id="ec284-568">hello UDF can also be used inside a filter as shown in hello example below, also qualified with hello "udf."</span></span> <span data-ttu-id="ec284-569">prefix:</span><span class="sxs-lookup"><span data-stu-id="ec284-569">prefix:</span></span>

<span data-ttu-id="ec284-570">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-570">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

<span data-ttu-id="ec284-571">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-571">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


<span data-ttu-id="ec284-572">I princip UDF: er är giltig skalära uttryck och kan användas i både projektioner och filter.</span><span class="sxs-lookup"><span data-stu-id="ec284-572">In essence, UDFs are valid scalar expressions and can be used in both projections and filters.</span></span> 

<span data-ttu-id="ec284-573">tooexpand på hello kraften i UDF: er, ska vi titta på ett annat exempel med villkorlig logik:</span><span class="sxs-lookup"><span data-stu-id="ec284-573">tooexpand on hello power of UDFs, let's look at another example with conditional logic:</span></span>

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


<span data-ttu-id="ec284-574">Nedan finns ett exempel som att övningarna hello UDF.</span><span class="sxs-lookup"><span data-stu-id="ec284-574">Below is an example that exercises hello UDF.</span></span>

<span data-ttu-id="ec284-575">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-575">**Query**</span></span>

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

<span data-ttu-id="ec284-576">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-576">**Results**</span></span>

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


<span data-ttu-id="ec284-577">Eftersom hello föregående exempel samlade integreras UDF: er hello kraften i JavaScript-språket med hello DocumentDB API SQL tooprovide en omfattande programmerbara gränssnitt toodo komplex procedurmässig, villkorlig logik med hello hjälp av inbyggda JavaScript-körning funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-577">As hello preceding examples showcase, UDFs integrate hello power of JavaScript language with hello DocumentDB API SQL tooprovide a rich programmable interface toodo complex procedural, conditional logic with hello help of inbuilt JavaScript runtime capabilities.</span></span>

<span data-ttu-id="ec284-578">DocumentDB API SQL här hello argument toohello UDF: er för varje dokument i hello källan hello aktuell etapp (WHERE-satsen eller SELECT-satsen) av bearbetning hello UDF.</span><span class="sxs-lookup"><span data-stu-id="ec284-578">DocumentDB API SQL provides hello arguments toohello UDFs for each document in hello source at hello current stage (WHERE clause or SELECT clause) of processing hello UDF.</span></span> <span data-ttu-id="ec284-579">hello resultatet är integrerat i hello övergripande körningspipeline sömlöst.</span><span class="sxs-lookup"><span data-stu-id="ec284-579">hello result is incorporated in hello overall execution pipeline seamlessly.</span></span> <span data-ttu-id="ec284-580">Om hello egenskaper som anges tooby hello UDF parametrar inte är tillgängliga i hello JSON-värde hello parametern betraktas som Odefinierad och därför hello ignoreras helt UDF-anrop.</span><span class="sxs-lookup"><span data-stu-id="ec284-580">If hello properties referred tooby hello UDF parameters are not available in hello JSON value, hello parameter is considered as undefined and hence hello UDF invocation is entirely skipped.</span></span> <span data-ttu-id="ec284-581">På liknande sätt om hello resultatet av hello UDF är odefinierad ingår den inte i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-581">Similarly if hello result of hello UDF is undefined, it's not included in hello result.</span></span> 

<span data-ttu-id="ec284-582">Sammanfattningsvis är UDF: er bra verktyg toodo komplicerad affärslogik som en del av hello frågan.</span><span class="sxs-lookup"><span data-stu-id="ec284-582">In summary, UDFs are great tools toodo complex business logic as part of hello query.</span></span>

### <a name="operator-evaluation"></a><span data-ttu-id="ec284-583">Operatorn utvärdering</span><span class="sxs-lookup"><span data-stu-id="ec284-583">Operator evaluation</span></span>
<span data-ttu-id="ec284-584">Cosmos DB, ritar bredd med JavaScript-operatörer och dess utvärdering semantik hello miljöpåverkan som JSON-databas.</span><span class="sxs-lookup"><span data-stu-id="ec284-584">Cosmos DB, by hello virtue of being a JSON database, draws parallels with JavaScript operators and its evaluation semantics.</span></span> <span data-ttu-id="ec284-585">Medan Cosmos DB försöker toopreserve JavaScript-semantik i JSON-support, avviker hello åtgärden utvärdering i vissa fall.</span><span class="sxs-lookup"><span data-stu-id="ec284-585">While Cosmos DB tries toopreserve JavaScript semantics in terms of JSON support, hello operation evaluation deviates in some instances.</span></span>

<span data-ttu-id="ec284-586">I DocumentDB API SQL, är till skillnad från i traditionella SQL hello typer av värden ofta inte känd tills hello värden hämtas från databasen.</span><span class="sxs-lookup"><span data-stu-id="ec284-586">In DocumentDB API SQL, unlike in traditional SQL, hello types of values are often not known until hello values are retrieved from database.</span></span> <span data-ttu-id="ec284-587">Köra frågor i ordning tooefficiently, de flesta av hello operatörer har strikt krav.</span><span class="sxs-lookup"><span data-stu-id="ec284-587">In order tooefficiently execute queries, most of hello operators have strict type requirements.</span></span> 

<span data-ttu-id="ec284-588">DocumentDB API SQL utförs inte implicita konverteringar, till skillnad från JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ec284-588">DocumentDB API SQL doesn't perform implicit conversions, unlike JavaScript.</span></span> <span data-ttu-id="ec284-589">Till exempel en fråga som `SELECT * FROM Person p WHERE p.Age = 21` matchar dokument som innehåller en ålder egenskap vars värde är 21.</span><span class="sxs-lookup"><span data-stu-id="ec284-589">For instance, a query like `SELECT * FROM Person p WHERE p.Age = 21` matches documents that contain an Age property whose value is 21.</span></span> <span data-ttu-id="ec284-590">Andra dokument vars ålder-egenskap stämmer med strängen ”21” eller andra eventuellt oändlig variationer som ”021”, ”21.0”, ”0021”, ”00021”, etc. matchas inte.</span><span class="sxs-lookup"><span data-stu-id="ec284-590">Any other document whose Age property matches string "21", or other possibly infinite variations like "021", "21.0", "0021", "00021", etc. will not be matched.</span></span> <span data-ttu-id="ec284-591">Detta är däremot toohello JavaScript där hello strängvärden är implicit omvandlas toonumbers (baserat på operator, ex: ==).</span><span class="sxs-lookup"><span data-stu-id="ec284-591">This is in contrast toohello JavaScript where hello string values are implicitly casted toonumbers (based on operator, ex: ==).</span></span> <span data-ttu-id="ec284-592">Det här alternativet är avgörande för effektiv index som matchar i DocumentDB API SQL.</span><span class="sxs-lookup"><span data-stu-id="ec284-592">This choice is crucial for efficient index matching in DocumentDB API SQL.</span></span> 

## <a name="parameterized-sql-queries"></a><span data-ttu-id="ec284-593">Parametriserade SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="ec284-593">Parameterized SQL queries</span></span>
<span data-ttu-id="ec284-594">Cosmos DB stöder frågor med parametrar uttryckt med hello bekant @ notation.</span><span class="sxs-lookup"><span data-stu-id="ec284-594">Cosmos DB supports queries with parameters expressed with hello familiar @ notation.</span></span> <span data-ttu-id="ec284-595">Parametriserade SQL ger stabil hantering och undantagstecken användarindata, förhindra oavsiktlig exponering av data via SQL injection.</span><span class="sxs-lookup"><span data-stu-id="ec284-595">Parameterized SQL provides robust handling and escaping of user input, preventing accidental exposure of data through SQL injection.</span></span> 

<span data-ttu-id="ec284-596">Du kan till exempel skriva en fråga som tar efternamn och region som parametrar och sedan exekverar för olika värden för efternamn och adressläge baserat på indata från användaren.</span><span class="sxs-lookup"><span data-stu-id="ec284-596">For example, you can write a query that takes last name and address state as parameters, and then execute it for various values of last name and address state based on user input.</span></span>

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

<span data-ttu-id="ec284-597">Denna begäran kan skicka tooCosmos DB som en JSON-fråga som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ec284-597">This request can then be sent tooCosmos DB as a parameterized JSON query like shown below.</span></span>

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

<span data-ttu-id="ec284-598">hello argumentet tooTOP kan anges med frågor med parametrar som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ec284-598">hello argument tooTOP can be set using parameterized queries like shown below.</span></span>

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

<span data-ttu-id="ec284-599">Parametervärden kan vara en giltig JSON (strängar, siffror, booleska värden, null, även matriser eller kapslade JSON).</span><span class="sxs-lookup"><span data-stu-id="ec284-599">Parameter values can be any valid JSON (strings, numbers, Booleans, null, even arrays or nested JSON).</span></span> <span data-ttu-id="ec284-600">Även eftersom Cosmos-DB-schema mindre, valideras parametrar inte mot alla typer.</span><span class="sxs-lookup"><span data-stu-id="ec284-600">Also since Cosmos DB is schema-less, parameters are not validated against any type.</span></span>

## <span data-ttu-id="ec284-601"><a id="BuiltinFunctions"></a>Inbyggda funktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-601"><a id="BuiltinFunctions"></a>Built-in functions</span></span>
<span data-ttu-id="ec284-602">Cosmos DB stöder också ett antal inbyggda funktioner för vanliga åtgärder som kan användas i frågor som användardefinierade funktioner (UDF).</span><span class="sxs-lookup"><span data-stu-id="ec284-602">Cosmos DB also supports a number of built-in functions for common operations, that can be used inside queries like user-defined functions (UDFs).</span></span>

| <span data-ttu-id="ec284-603">Funktionen grupp</span><span class="sxs-lookup"><span data-stu-id="ec284-603">Function group</span></span>          | <span data-ttu-id="ec284-604">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="ec284-604">Operations</span></span>                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ec284-605">Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-605">Mathematical functions</span></span>  | <span data-ttu-id="ec284-606">ABS, TAKET, EXP, VÅNING, LOG, LOG10, POWER, runda, LOGGA, SQRT, ruta, AVKORTA, ARCCOS, ARCSIN, ARCTAN, ATN2, COS, bet, grader, PI, radianer, SIN och TAN</span><span class="sxs-lookup"><span data-stu-id="ec284-606">ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN, and TAN</span></span> |
| <span data-ttu-id="ec284-607">Ange kontrollerar funktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-607">Type checking functions</span></span> | <span data-ttu-id="ec284-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED och IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="ec284-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, and IS_PRIMITIVE</span></span>                                                           |
| <span data-ttu-id="ec284-609">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-609">String functions</span></span>        | <span data-ttu-id="ec284-610">CONCAT, innehåller, ENDSWITH, INDEX_OF, vänster, längd, nedre, LTRIM, Ersätt, REPLIKERA, OMVÄND, höger, RTRIM, STARTSWITH, DELSTRÄNGEN och övre</span><span class="sxs-lookup"><span data-stu-id="ec284-610">CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, and UPPER</span></span>       |
| <span data-ttu-id="ec284-611">Matrisfunktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-611">Array functions</span></span>         | <span data-ttu-id="ec284-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH och ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="ec284-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH, and ARRAY_SLICE</span></span>                                                                                         |
| <span data-ttu-id="ec284-613">Spatial funktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-613">Spatial functions</span></span>       | <span data-ttu-id="ec284-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID och ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="ec284-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, and ST_ISVALIDDETAILED</span></span>                                                                           | 

<span data-ttu-id="ec284-615">Om du använder en användardefinierad funktion (UDF) som en inbyggd funktion är nu tillgänglig, bör du använda hello motsvarande inbyggd funktion som det ska toobe snabbare toorun och mer effektivt.</span><span class="sxs-lookup"><span data-stu-id="ec284-615">If you’re currently using a user-defined function (UDF) for which a built-in function is now available, you should use hello corresponding built-in function as it is going toobe quicker toorun and more efficiently.</span></span> 

### <a name="mathematical-functions"></a><span data-ttu-id="ec284-616">Matematiska funktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-616">Mathematical functions</span></span>
<span data-ttu-id="ec284-617">hello matematiska funktioner varje utför en beräkning, baserat på värden som har angetts som argument och returnerar ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-617">hello mathematical functions each perform a calculation, based on input values that are provided as arguments, and return a numeric value.</span></span> <span data-ttu-id="ec284-618">Här är en tabell med inbyggda matematiska funktioner som stöds.</span><span class="sxs-lookup"><span data-stu-id="ec284-618">Here’s a table of supported built-in mathematical functions.</span></span>


| <span data-ttu-id="ec284-619">Användning</span><span class="sxs-lookup"><span data-stu-id="ec284-619">Usage</span></span> | <span data-ttu-id="ec284-620">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec284-620">Description</span></span> |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ec284-621">[[ABS (num_expr)](#bk_abs)</span><span class="sxs-lookup"><span data-stu-id="ec284-621">[[ABS (num_expr)](#bk_abs)</span></span> | <span data-ttu-id="ec284-622">Returnerar hello absolut (positivt) värdet för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-622">Returns hello absolute (positive) value of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-623">TAKET (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-623">CEILING (num_expr)</span></span>](#bk_ceiling) | <span data-ttu-id="ec284-624">Returnerar hello minsta heltal som är större än eller lika med för att hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-624">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-625">VÅNING (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-625">FLOOR (num_expr)</span></span>](#bk_floor) | <span data-ttu-id="ec284-626">Returnerar hello största heltal som är mindre än eller lika toohello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-626">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-627">EXP (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-627">EXP (num_expr)</span></span>](#bk_exp) | <span data-ttu-id="ec284-628">Returnerar hello e upphöjt till hello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-628">Returns hello exponent of hello specified numeric expression.</span></span> |
| <span data-ttu-id="ec284-629">[LOGGEN (num_expr [, base])](#bk_log)</span><span class="sxs-lookup"><span data-stu-id="ec284-629">[LOG (num_expr [,base])](#bk_log)</span></span> | <span data-ttu-id="ec284-630">Returnerar hello naturliga logaritmen för hello angetts numeriskt uttryck eller hello logaritmen med hello angetts bas</span><span class="sxs-lookup"><span data-stu-id="ec284-630">Returns hello natural logarithm of hello specified numeric expression, or hello logarithm using hello specified base</span></span> |
| [<span data-ttu-id="ec284-631">Log10 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-631">LOG10 (num_expr)</span></span>](#bk_log10) | <span data-ttu-id="ec284-632">Returnerar hello 10-logaritmiska värdet för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-632">Returns hello base-10 logarithmic value of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-633">AVRUNDA (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-633">ROUND (num_expr)</span></span>](#bk_round) | <span data-ttu-id="ec284-634">Returnerar ett numeriskt värde avrundade toohello närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="ec284-634">Returns a numeric value, rounded toohello closest integer value.</span></span> |
| [<span data-ttu-id="ec284-635">AVKORTA (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-635">TRUNC (num_expr)</span></span>](#bk_trunc) | <span data-ttu-id="ec284-636">Returnerar ett numeriskt värde, trunkerat toohello närmaste heltal.</span><span class="sxs-lookup"><span data-stu-id="ec284-636">Returns a numeric value, truncated toohello closest integer value.</span></span> |
| [<span data-ttu-id="ec284-637">SQRT (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-637">SQRT (num_expr)</span></span>](#bk_sqrt) | <span data-ttu-id="ec284-638">Returnerar hello kvadratroten ur hello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-638">Returns hello square root of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-639">RUTA (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-639">SQUARE (num_expr)</span></span>](#bk_square) | <span data-ttu-id="ec284-640">Returnerar hello rektangulär hello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-640">Returns hello square of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-641">STRÖMFÖRSÖRJNING (num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-641">POWER (num_expr, num_expr)</span></span>](#bk_power) | <span data-ttu-id="ec284-642">Returnerar hello kraften hos hello angetts stränguttryck toohello värde har angetts.</span><span class="sxs-lookup"><span data-stu-id="ec284-642">Returns hello power of hello specified numeric expression toohello value specified.</span></span> |
| [<span data-ttu-id="ec284-643">LOGGA (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-643">SIGN (num_expr)</span></span>](#bk_sign) | <span data-ttu-id="ec284-644">Returnerar hello logga värdet (-1, 0, 1) för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-644">Returns hello sign value (-1, 0, 1) of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-645">ARCCOS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-645">ACOS (num_expr)</span></span>](#bk_acos) | <span data-ttu-id="ec284-646">Returnerar hello vinkel angivna i radianer, vars cosinus är hello numeriska uttrycket; kallas även cosinus.</span><span class="sxs-lookup"><span data-stu-id="ec284-646">Returns hello angle, in radians, whose cosine is hello specified numeric expression; also called arccosine.</span></span> |
| [<span data-ttu-id="ec284-647">ARCSIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-647">ASIN (num_expr)</span></span>](#bk_asin) | <span data-ttu-id="ec284-648">Returnerar hello vinkel angivna i radianer, vars sinus är hello numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-648">Returns hello angle, in radians, whose sine is hello specified numeric expression.</span></span> <span data-ttu-id="ec284-649">Detta kallas också arcsinus.</span><span class="sxs-lookup"><span data-stu-id="ec284-649">This is also called arcsine.</span></span> |
| [<span data-ttu-id="ec284-650">ARCTAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-650">ATAN (num_expr)</span></span>](#bk_atan) | <span data-ttu-id="ec284-651">Returnerar hello vinkel angivna i radianer, vars tangens är hello numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-651">Returns hello angle, in radians, whose tangent is hello specified numeric expression.</span></span> <span data-ttu-id="ec284-652">Detta kallas också tangens.</span><span class="sxs-lookup"><span data-stu-id="ec284-652">This is also called arctangent.</span></span> |
| [<span data-ttu-id="ec284-653">ATN2 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-653">ATN2 (num_expr)</span></span>](#bk_atn2) | <span data-ttu-id="ec284-654">Returnerar hello vinkel i radianer mellan hello positiva x-axeln och hello ray från hello toohello origo (y, x), där x och y är hello värdena för hello två angivna float uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-654">Returns hello angle, in radians, between hello positive x-axis and hello ray from hello origin toohello point (y, x), where x and y are hello values of hello two specified float expressions.</span></span> |
| [<span data-ttu-id="ec284-655">COS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-655">COS (num_expr)</span></span>](#bk_cos) | <span data-ttu-id="ec284-656">Returnerar hello trigonometriska värden cosinus för hello angiven vinkel i radianer, i hello angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-656">Returns hello trigonometric cosine of hello specified angle, in radians, in hello specified expression.</span></span> |
| [<span data-ttu-id="ec284-657">BET (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-657">COT (num_expr)</span></span>](#bk_cot) | <span data-ttu-id="ec284-658">Returnerar hello trigonometriska värden cotangens för hello angiven vinkel i radianer, i hello anges numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-658">Returns hello trigonometric cotangent of hello specified angle, in radians, in hello specified numeric expression.</span></span> |
| [<span data-ttu-id="ec284-659">GRADER (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-659">DEGREES (num_expr)</span></span>](#bk_degrees) | <span data-ttu-id="ec284-660">Returnerar hello motsvarande vinkel i grader för en vinkel angiven i radianer.</span><span class="sxs-lookup"><span data-stu-id="ec284-660">Returns hello corresponding angle in degrees for an angle specified in radians.</span></span> |
| [<span data-ttu-id="ec284-661">PI)</span><span class="sxs-lookup"><span data-stu-id="ec284-661">PI ()</span></span>](#bk_pi) | <span data-ttu-id="ec284-662">Returnerar hello konstantvärde pi.</span><span class="sxs-lookup"><span data-stu-id="ec284-662">Returns hello constant value of PI.</span></span> |
| [<span data-ttu-id="ec284-663">RADIANER (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-663">RADIANS (num_expr)</span></span>](#bk_radians) | <span data-ttu-id="ec284-664">Returnerar radianer när ett numeriskt uttryck i grader anges.</span><span class="sxs-lookup"><span data-stu-id="ec284-664">Returns radians when a numeric expression, in degrees, is entered.</span></span> |
| [<span data-ttu-id="ec284-665">SIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-665">SIN (num_expr)</span></span>](#bk_sin) | <span data-ttu-id="ec284-666">Returnerar hello trigonometriska värden sinus för hello angiven vinkel i radianer, i hello angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-666">Returns hello trigonometric sine of hello specified angle, in radians, in hello specified expression.</span></span> |
| [<span data-ttu-id="ec284-667">TAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-667">TAN (num_expr)</span></span>](#bk_tan) | <span data-ttu-id="ec284-668">Returnerar hello tangens för hello inkommande uttryck i hello angivna uttrycket.</span><span class="sxs-lookup"><span data-stu-id="ec284-668">Returns hello tangent of hello input expression, in hello specified expression.</span></span> |

<span data-ttu-id="ec284-669">Du kan till exempel nu köra frågor som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="ec284-669">For example, you can now run queries like hello following:</span></span>

<span data-ttu-id="ec284-670">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-670">**Query**</span></span>

    SELECT VALUE ABS(-4)

<span data-ttu-id="ec284-671">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-671">**Results**</span></span>

    [4]

<span data-ttu-id="ec284-672">hello största skillnaden mellan Cosmos DB funktioner jämfört med tooANSI SQL är att de är utformad toowork bra med schemat mindre och blandad schemadata.</span><span class="sxs-lookup"><span data-stu-id="ec284-672">hello main difference between Cosmos DB’s functions compared tooANSI SQL is that they are designed toowork well with schema-less and mixed schema data.</span></span> <span data-ttu-id="ec284-673">Till exempel om du har ett dokument där egenskapen hello saknas eller har ett icke-numeriska värde som ”okänt” och sedan hello dokumentet hoppas över, i stället för att returnera ett fel.</span><span class="sxs-lookup"><span data-stu-id="ec284-673">For example, if you have a document where hello Size property is missing, or has a non-numeric value like “unknown”, then hello document is skipped over, instead of returning an error.</span></span>

### <a name="type-checking-functions"></a><span data-ttu-id="ec284-674">Ange kontrollerar funktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-674">Type checking functions</span></span>
<span data-ttu-id="ec284-675">hello typ kontrollerar funktioner kan du toocheck hello typ av ett uttryck i SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-675">hello type checking functions allow you toocheck hello type of an expression within SQL queries.</span></span> <span data-ttu-id="ec284-676">Typen som kontrollerar funktioner kan inte användas toodetermine hello typ av egenskaper i dokument på hello direkt när den variabel eller okänd.</span><span class="sxs-lookup"><span data-stu-id="ec284-676">Type checking functions can be used toodetermine hello type of properties within documents on hello fly when it is variable or unknown.</span></span> <span data-ttu-id="ec284-677">Här är en tabell med stöds inbyggd typ kontrollerar funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-677">Here’s a table of supported built-in type checking functions.</span></span>

<table>
<tr>
  <td><span data-ttu-id="ec284-678"><strong>Användning</strong></span><span class="sxs-lookup"><span data-stu-id="ec284-678"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="ec284-679"><strong>Beskrivning</strong></span><span class="sxs-lookup"><span data-stu-id="ec284-679"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-681">Returnerar ett booleskt värde som anger om hello typ av hello värde är en matris.</span><span class="sxs-lookup"><span data-stu-id="ec284-681">Returns a Boolean indicating if hello type of hello value is an array.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-683">Returnerar ett booleskt värde som anger om hello typ av hello värde är ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-683">Returns a Boolean indicating if hello type of hello value is a Boolean.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-685">Returnerar ett booleskt värde som anger om hello typ av hello värde är null.</span><span class="sxs-lookup"><span data-stu-id="ec284-685">Returns a Boolean indicating if hello type of hello value is null.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-687">Returnerar ett booleskt värde som anger om hello typ av hello värde är ett tal.</span><span class="sxs-lookup"><span data-stu-id="ec284-687">Returns a Boolean indicating if hello type of hello value is a number.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-689">Returnerar ett booleskt värde som anger om hello typ av hello värde är ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="ec284-689">Returns a Boolean indicating if hello type of hello value is a JSON object.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-691">Returnerar ett booleskt värde som anger om hello typ av hello värde är en sträng.</span><span class="sxs-lookup"><span data-stu-id="ec284-691">Returns a Boolean indicating if hello type of hello value is a string.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-693">Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-693">Returns a Boolean indicating if hello property has been assigned a value.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (uttryck)</a></span><span class="sxs-lookup"><span data-stu-id="ec284-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></span></span></td>
  <td><span data-ttu-id="ec284-695">Returnerar ett booleskt värde som anger om hello typ av hello värde är en sträng, siffra, Boolean eller null.</span><span class="sxs-lookup"><span data-stu-id="ec284-695">Returns a Boolean indicating if hello type of hello value is a string, number, Boolean or null.</span></span></td>
</tr>

</table>

<span data-ttu-id="ec284-696">Med dessa funktioner kan köra du nu frågor som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="ec284-696">Using these functions, you can now run queries like hello following:</span></span>

<span data-ttu-id="ec284-697">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-697">**Query**</span></span>

    SELECT VALUE IS_NUMBER(-4)

<span data-ttu-id="ec284-698">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-698">**Results**</span></span>

    [true]

### <a name="string-functions"></a><span data-ttu-id="ec284-699">Strängfunktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-699">String functions</span></span>
<span data-ttu-id="ec284-700">hello följande skalärfunktioner utföra en åtgärd på ett inkommande värde och returnerar en sträng, numeriskt eller booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-700">hello following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span> <span data-ttu-id="ec284-701">Här är en tabell med inbyggda strängfunktioner:</span><span class="sxs-lookup"><span data-stu-id="ec284-701">Here's a table of built-in string functions:</span></span>

| <span data-ttu-id="ec284-702">Användning</span><span class="sxs-lookup"><span data-stu-id="ec284-702">Usage</span></span> | <span data-ttu-id="ec284-703">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec284-703">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="ec284-704">LÄNGDEN (str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-704">LENGTH (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |<span data-ttu-id="ec284-705">Returnerar hello antalet tecken i hello angetts stränguttryck</span><span class="sxs-lookup"><span data-stu-id="ec284-705">Returns hello number of characters of hello specified string expression</span></span> |
| <span data-ttu-id="ec284-706">[SAMMANFOGA (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span><span class="sxs-lookup"><span data-stu-id="ec284-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span></span> |<span data-ttu-id="ec284-707">Returnerar en sträng som är hello resultat sammanfoga två eller flera strängvärden.</span><span class="sxs-lookup"><span data-stu-id="ec284-707">Returns a string that is hello result of concatenating two or more string values.</span></span> |
| [<span data-ttu-id="ec284-708">DELSTRÄNGEN (str_expr, num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-708">SUBSTRING (str_expr, num_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |<span data-ttu-id="ec284-709">Returnerar en del av ett stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-709">Returns part of a string expression.</span></span> |
| [<span data-ttu-id="ec284-710">STARTSWITH (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-710">STARTSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |<span data-ttu-id="ec284-711">Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra</span><span class="sxs-lookup"><span data-stu-id="ec284-711">Returns a Boolean indicating whether hello first string expression ends with hello second</span></span> |
| [<span data-ttu-id="ec284-712">ENDSWITH (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-712">ENDSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |<span data-ttu-id="ec284-713">Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra</span><span class="sxs-lookup"><span data-stu-id="ec284-713">Returns a Boolean indicating whether hello first string expression ends with hello second</span></span> |
| [<span data-ttu-id="ec284-714">INNEHÅLLER (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-714">CONTAINS (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |<span data-ttu-id="ec284-715">Returnerar ett booleskt värde som anger om hello första stränguttryck innehåller hello andra.</span><span class="sxs-lookup"><span data-stu-id="ec284-715">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span> |
| [<span data-ttu-id="ec284-716">INDEX_OF (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-716">INDEX_OF (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |<span data-ttu-id="ec284-717">Returnerar hello startposition för hello första förekomsten av hello andra stränguttryck inom hello första angivet stränguttryck eller -1 om hello strängen inte hittas.</span><span class="sxs-lookup"><span data-stu-id="ec284-717">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span> |
| [<span data-ttu-id="ec284-718">VÄNSTER (str_expr num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-718">LEFT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |<span data-ttu-id="ec284-719">Returnerar hello vänstra del av en sträng med hello angivna antalet tecken.</span><span class="sxs-lookup"><span data-stu-id="ec284-719">Returns hello left part of a string with hello specified number of characters.</span></span> |
| [<span data-ttu-id="ec284-720">RIGHT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-720">RIGHT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |<span data-ttu-id="ec284-721">Returnerar hello just en del av en sträng med hello angivet antal tecken.</span><span class="sxs-lookup"><span data-stu-id="ec284-721">Returns hello right part of a string with hello specified number of characters.</span></span> |
| [<span data-ttu-id="ec284-722">LTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-722">LTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |<span data-ttu-id="ec284-723">Returnerar ett stränguttryck när den tar bort inledande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="ec284-723">Returns a string expression after it removes leading blanks.</span></span> |
| [<span data-ttu-id="ec284-724">RTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-724">RTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |<span data-ttu-id="ec284-725">Returnerar ett stränguttryck efter trunkering av alla avslutande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="ec284-725">Returns a string expression after truncating all trailing blanks.</span></span> |
| [<span data-ttu-id="ec284-726">NEDRE (str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-726">LOWER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |<span data-ttu-id="ec284-727">Returnerar ett stränguttryck efter konvertering versal data toolowercase.</span><span class="sxs-lookup"><span data-stu-id="ec284-727">Returns a string expression after converting uppercase character data toolowercase.</span></span> |
| [<span data-ttu-id="ec284-728">ÖVRE (str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-728">UPPER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |<span data-ttu-id="ec284-729">Returnerar ett stränguttryck efter konvertering gemen data toouppercase.</span><span class="sxs-lookup"><span data-stu-id="ec284-729">Returns a string expression after converting lowercase character data toouppercase.</span></span> |
| [<span data-ttu-id="ec284-730">Ersätt (str_expr, str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-730">REPLACE (str_expr, str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |<span data-ttu-id="ec284-731">Ersätter alla förekomster av ett angivet strängvärde med ett annat värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-731">Replaces all occurrences of a specified string value with another string value.</span></span> |
| [<span data-ttu-id="ec284-732">REPLIKERA (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-732">REPLICATE (str_expr, num_expr)</span></span>](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |<span data-ttu-id="ec284-733">Upprepar ett strängvärde till ett angivet antal gånger.</span><span class="sxs-lookup"><span data-stu-id="ec284-733">Repeats a string value a specified number of times.</span></span> |
| [<span data-ttu-id="ec284-734">OMVÄND (str_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-734">REVERSE (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |<span data-ttu-id="ec284-735">Returnerar hello omvänd ordning i ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="ec284-735">Returns hello reverse order of a string value.</span></span> |

<span data-ttu-id="ec284-736">Med dessa funktioner kan köra du nu frågor som hello nedan.</span><span class="sxs-lookup"><span data-stu-id="ec284-736">Using these functions, you can now run queries like hello following.</span></span> <span data-ttu-id="ec284-737">Exempelvis returnerar hello efternamn versaler på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ec284-737">For example, you can return hello family name in uppercase as follows:</span></span>

<span data-ttu-id="ec284-738">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-738">**Query**</span></span>

    SELECT VALUE UPPER(Families.id)
    FROM Families

<span data-ttu-id="ec284-739">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-739">**Results**</span></span>

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

<span data-ttu-id="ec284-740">Eller sammanfoga strängar som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="ec284-740">Or concatenate strings like in this example:</span></span>

<span data-ttu-id="ec284-741">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-741">**Query**</span></span>

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

<span data-ttu-id="ec284-742">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-742">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


<span data-ttu-id="ec284-743">Strängfunktioner kan också användas i hello där satsen toofilter resultat, precis som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ec284-743">String functions can also be used in hello WHERE clause toofilter results, like in hello following example:</span></span>

<span data-ttu-id="ec284-744">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-744">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

<span data-ttu-id="ec284-745">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-745">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a><span data-ttu-id="ec284-746">Matrisfunktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-746">Array functions</span></span>
<span data-ttu-id="ec284-747">hello följande skalärfunktioner utföra en åtgärd på en matris indatavärdet och returnera numeriska, Boolean eller matrisen värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-747">hello following scalar functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span> <span data-ttu-id="ec284-748">Här är en tabell med inbyggda matrisfunktioner:</span><span class="sxs-lookup"><span data-stu-id="ec284-748">Here's a table of built-in array functions:</span></span>

| <span data-ttu-id="ec284-749">Användning</span><span class="sxs-lookup"><span data-stu-id="ec284-749">Usage</span></span> | <span data-ttu-id="ec284-750">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec284-750">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="ec284-751">ARRAY_LENGTH (arr_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-751">ARRAY_LENGTH (arr_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |<span data-ttu-id="ec284-752">Returnerar hello antalet element i hello angetts matrisuttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-752">Returns hello number of elements of hello specified array expression.</span></span> |
| <span data-ttu-id="ec284-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span><span class="sxs-lookup"><span data-stu-id="ec284-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span></span> |<span data-ttu-id="ec284-754">Returnerar en matris som är hello resultat sammanfoga två eller flera matrisvärden.</span><span class="sxs-lookup"><span data-stu-id="ec284-754">Returns an array that is hello result of concatenating two or more array values.</span></span> |
| <span data-ttu-id="ec284-755">[ARRAY_CONTAINS (arr_expr uttryck [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span><span class="sxs-lookup"><span data-stu-id="ec284-755">[ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span></span> |<span data-ttu-id="ec284-756">Returnerar ett booleskt värde som anger om hello matris innehåller hello anges värdet.</span><span class="sxs-lookup"><span data-stu-id="ec284-756">Returns a Boolean indicating whether hello array contains hello specified value.</span></span> <span data-ttu-id="ec284-757">Ange om hello matchning är fullständigt eller partiellt.</span><span class="sxs-lookup"><span data-stu-id="ec284-757">Can specify if hello match is full or partial.</span></span> |
| <span data-ttu-id="ec284-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span><span class="sxs-lookup"><span data-stu-id="ec284-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span></span> |<span data-ttu-id="ec284-759">Returnerar en del av ett matrisuttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-759">Returns part of an array expression.</span></span> |

<span data-ttu-id="ec284-760">Matrisfunktioner kan inte använda toomanipulate matriser i JSON.</span><span class="sxs-lookup"><span data-stu-id="ec284-760">Array functions can be used toomanipulate arrays within JSON.</span></span> <span data-ttu-id="ec284-761">Här är till exempel en fråga som returnerar alla dokument där en av hello överordnade är ”Robin Wakefield”.</span><span class="sxs-lookup"><span data-stu-id="ec284-761">For example, here's a query that returns all documents where one of hello parents is "Robin Wakefield".</span></span> 

<span data-ttu-id="ec284-762">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-762">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

<span data-ttu-id="ec284-763">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-763">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="ec284-764">Du kan ange ett partiellt fragment för matchande element inom hello matris.</span><span class="sxs-lookup"><span data-stu-id="ec284-764">You can specify a partial fragment for matching elements within hello array.</span></span> <span data-ttu-id="ec284-765">hello följande exempelfråga hittar du alla överordnade med hello `givenName` av `Robin`.</span><span class="sxs-lookup"><span data-stu-id="ec284-765">hello following query finds all parents with hello `givenName` of `Robin`.</span></span>

<span data-ttu-id="ec284-766">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-766">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

<span data-ttu-id="ec284-767">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-767">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]


<span data-ttu-id="ec284-768">Här är ett annat exempel som använder ARRAY_LENGTH tooget hello antalet underordnade per familj.</span><span class="sxs-lookup"><span data-stu-id="ec284-768">Here's another example that uses ARRAY_LENGTH tooget hello number of children per family.</span></span>

<span data-ttu-id="ec284-769">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-769">**Query**</span></span>

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

<span data-ttu-id="ec284-770">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-770">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a><span data-ttu-id="ec284-771">Spatial funktioner</span><span class="sxs-lookup"><span data-stu-id="ec284-771">Spatial functions</span></span>
<span data-ttu-id="ec284-772">Cosmos DB stöder hello följande öppna geospatiala Consortium (OGC) inbyggda funktioner för geospatial frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-772">Cosmos DB supports hello following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> 

<table>
<tr>
  <td><span data-ttu-id="ec284-773"><strong>Användning</strong></span><span class="sxs-lookup"><span data-stu-id="ec284-773"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="ec284-774"><strong>Beskrivning</strong></span><span class="sxs-lookup"><span data-stu-id="ec284-774"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-775">ST_DISTANCE (point_expr, point_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-775">ST_DISTANCE (point_expr, point_expr)</span></span></td>
  <td><span data-ttu-id="ec284-776">Returnerar hello avståndet mellan hello två GeoJSON punkt, Polygon eller LineString uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-776">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-777">ST_WITHIN (point_expr, polygon_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-777">ST_WITHIN (point_expr, polygon_expr)</span></span></td>
  <td><span data-ttu-id="ec284-778">Returnerar ett booleskt uttryck som anger om hello första GeoJSON objekt (Point, LineString eller Polygon) är i hello andra GeoJSON objekt (Point, LineString eller Polygon).</span><span class="sxs-lookup"><span data-stu-id="ec284-778">Returns a Boolean expression indicating whether hello first GeoJSON object (Point, Polygon, or LineString) is within hello second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="ec284-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="ec284-780">Returnerar ett booleskt uttryck som anger om intersect hello två angivna GeoJSON-objekt (Point, LineString eller Polygon).</span><span class="sxs-lookup"><span data-stu-id="ec284-780">Returns a Boolean expression indicating whether hello two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-781">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="ec284-781">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="ec284-782">Returnerar ett booleskt värde som anger om hello angiven GeoJSON punkt, Polygon eller LineString uttryck är giltig.</span><span class="sxs-lookup"><span data-stu-id="ec284-782">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="ec284-783">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="ec284-783">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="ec284-784">Returnerar ett JSON-värde som innehåller ett booleskt värde om hello angivna GeoJSON punkt, Polygon eller LineString uttrycket är giltig och om det är ogiltig, dessutom hello orsak som ett strängvärde.</span><span class="sxs-lookup"><span data-stu-id="ec284-784">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="ec284-785">Spatial funktioner kan inte använda tooperform närhet frågor mot spatialdata.</span><span class="sxs-lookup"><span data-stu-id="ec284-785">Spatial functions can be used tooperform proximity queries against spatial data.</span></span> <span data-ttu-id="ec284-786">Här är till exempel en fråga som returnerar alla familj dokument att 30 km av hello angivna platsen använder hello ST_DISTANCE inbyggd funktion.</span><span class="sxs-lookup"><span data-stu-id="ec284-786">For example, here's a query that returns all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="ec284-787">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="ec284-787">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="ec284-788">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-788">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="ec284-789">Mer information om geospatiala stöds i Cosmos-databasen finns [arbeta med geospatiala data i Azure Cosmos DB](geospatial.md).</span><span class="sxs-lookup"><span data-stu-id="ec284-789">For more details on geospatial support in Cosmos DB, please see [Working with geospatial data in Azure Cosmos DB](geospatial.md).</span></span> <span data-ttu-id="ec284-790">Som packar upp spatial funktioner och hello SQL-syntax för Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ec284-790">That wraps up spatial functions, and hello SQL syntax for Cosmos DB.</span></span> <span data-ttu-id="ec284-791">Nu ska vi ta en titt på hur LINQ-frågor fungerar och hur den interagerar med hello syntax vi sett hittills.</span><span class="sxs-lookup"><span data-stu-id="ec284-791">Now let's take a look at how LINQ querying works and how it interacts with hello syntax we've seen so far.</span></span>

## <span data-ttu-id="ec284-792"><a id="Linq"></a>LINQ tooDocumentDB API SQL</span><span class="sxs-lookup"><span data-stu-id="ec284-792"><a id="Linq"></a>LINQ tooDocumentDB API SQL</span></span>
<span data-ttu-id="ec284-793">LINQ är en programmeringsmodell för .NET som representerar beräkning som frågor för dataströmmar med objekt.</span><span class="sxs-lookup"><span data-stu-id="ec284-793">LINQ is a .NET programming model that expresses computation as queries on streams of objects.</span></span> <span data-ttu-id="ec284-794">Cosmos DB tillhandahåller en klientsidan biblioteket toointerface med LINQ genom att underlätta konvertering mellan JSON och .NET-objekt och mappning från en delmängd av LINQ frågor tooCosmos DB frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-794">Cosmos DB provides a client-side library toointerface with LINQ by facilitating a conversion between JSON and .NET objects and a mapping from a subset of LINQ queries tooCosmos DB queries.</span></span> 

<span data-ttu-id="ec284-795">hello bilden nedan visar hello-arkitektur stöder LINQ-frågor med Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="ec284-795">hello picture below shows hello architecture of supporting LINQ queries using Cosmos DB.</span></span>  <span data-ttu-id="ec284-796">Med hello Cosmos-DB-klient kan utvecklare skapa ett **IQueryable** objekt att direkt frågor hello fråga Cosmos-DB-provider, som sedan översätter hello LINQ-fråga till en Cosmos-DB-fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-796">Using hello Cosmos DB client, developers can create an **IQueryable** object that directly queries hello Cosmos DB query provider, which then translates hello LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="ec284-797">hello fråga skickas sedan toohello Cosmos DB server tooretrieve en uppsättning resultat i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="ec284-797">hello query is then passed toohello Cosmos DB server tooretrieve a set of results in JSON format.</span></span> <span data-ttu-id="ec284-798">hello returnerade resultatet är avserialiserat till en dataström med .NET-objekt på hello på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="ec284-798">hello returned results are deserialized into a stream of .NET objects on hello client side.</span></span>

![Arkitektur för LINQ-frågor med DocumentDB-API: T - SQL-syntaxen, JSON-frågespråket, databasbegrepp och SQL-frågor][1]

### <a name="net-and-json-mapping"></a><span data-ttu-id="ec284-800">.NET och JSON-mappning</span><span class="sxs-lookup"><span data-stu-id="ec284-800">.NET and JSON mapping</span></span>
<span data-ttu-id="ec284-801">hello mappning mellan .NET-objekt och JSON-dokument är naturlig - varje medlem datafält mappas tooa JSON-objekt, där hello fältnamnet är mappad toohello ”nyckeln” en del av hello objekt och hello ”värde” del tillhör rekursivt mappas toohello värdet hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="ec284-801">hello mapping between .NET objects and JSON documents is natural - each data member field is mapped tooa JSON object, where hello field name is mapped toohello "key" part of hello object and hello "value" part is recursively mapped toohello value part of hello object.</span></span> <span data-ttu-id="ec284-802">Överväg följande exempel hello: hello familj objekt som skapas är mappade toohello JSON-dokumentet som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ec284-802">Consider hello following example: hello Family object created is mapped toohello JSON document as shown below.</span></span> <span data-ttu-id="ec284-803">Och vice versa hello JSON-dokumentet är mappade tillbaka tooa .NET-objekt.</span><span class="sxs-lookup"><span data-stu-id="ec284-803">And vice versa, hello JSON document is mapped back tooa .NET object.</span></span>

<span data-ttu-id="ec284-804">**C#-klass**</span><span class="sxs-lookup"><span data-stu-id="ec284-804">**C# Class**</span></span>

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


<span data-ttu-id="ec284-805">**JSON**</span><span class="sxs-lookup"><span data-stu-id="ec284-805">**JSON**</span></span>  

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



### <a name="linq-toosql-translation"></a><span data-ttu-id="ec284-806">LINQ tooSQL översättning</span><span class="sxs-lookup"><span data-stu-id="ec284-806">LINQ tooSQL translation</span></span>
<span data-ttu-id="ec284-807">Hej Cosmos frågan databasleverantör utför en bästa prestanda-mappning från en LINQ-fråga till en Cosmos-Databasens SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-807">hello Cosmos DB query provider performs a best effort mapping from a LINQ query into a Cosmos DB SQL query.</span></span> <span data-ttu-id="ec284-808">I följande beskrivning hello, förutsätter vi att du hello läsaren har en grundläggande kunskaper inom LINQ.</span><span class="sxs-lookup"><span data-stu-id="ec284-808">In hello following description, we assume hello reader has a basic familiarity of LINQ.</span></span>

<span data-ttu-id="ec284-809">Vi kan först stöder alla JSON primitiva typer – numeriska typer, boolean, sträng eller null för hello typsystemet.</span><span class="sxs-lookup"><span data-stu-id="ec284-809">First, for hello type system, we support all JSON primitive types – numeric types, boolean, string, and null.</span></span> <span data-ttu-id="ec284-810">Endast typerna JSON stöds.</span><span class="sxs-lookup"><span data-stu-id="ec284-810">Only these JSON types are supported.</span></span> <span data-ttu-id="ec284-811">följande skaläruttryck hello stöds.</span><span class="sxs-lookup"><span data-stu-id="ec284-811">hello following scalar expressions are supported.</span></span>

* <span data-ttu-id="ec284-812">Konstanta värden – dessa inkluderar konstanta värden hello primitiva datatyper för närvarande hello hello frågan utvärderas.</span><span class="sxs-lookup"><span data-stu-id="ec284-812">Constant values – these include constant values of hello primitive data types at hello time hello query is evaluated.</span></span>
* <span data-ttu-id="ec284-813">Egenskapen/matrisen indexuttryck – dessa uttryck finns toohello-egenskapen för ett objekt eller ett matriselement.</span><span class="sxs-lookup"><span data-stu-id="ec284-813">Property/array index expressions – these expressions refer toohello property of an object or an array element.</span></span>
  
     <span data-ttu-id="ec284-814">familj. ID;    Family.children[0].familyName;    Family.children[0].grade;    Family.children[n].grade; n är en int-variabel</span><span class="sxs-lookup"><span data-stu-id="ec284-814">family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n is an int variable</span></span>
* <span data-ttu-id="ec284-815">Aritmetiska uttryck - dessa inkluderar vanliga aritmetiska uttryck på numeriska och booleska värden.</span><span class="sxs-lookup"><span data-stu-id="ec284-815">Arithmetic expressions - These include common arithmetic expressions on numerical and boolean values.</span></span> <span data-ttu-id="ec284-816">Hello fullständig lista finns i toohello SQL-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="ec284-816">For hello complete list, refer toohello SQL specification.</span></span>
  
     <span data-ttu-id="ec284-817">2 * family.children[0].grade;    x + y;</span><span class="sxs-lookup"><span data-stu-id="ec284-817">2 * family.children[0].grade;    x + y;</span></span>
* <span data-ttu-id="ec284-818">Jämförelse av stränguttryck - bland annat att jämföra ett värde toosome konstantsträng strängvärde.</span><span class="sxs-lookup"><span data-stu-id="ec284-818">String comparison expression - these include comparing a string value toosome constant string value.</span></span>  
  
     <span data-ttu-id="ec284-819">mother.familyName == ”Smith”;    child.givenName == s. s är en string-variabel</span><span class="sxs-lookup"><span data-stu-id="ec284-819">mother.familyName == "Smith";    child.givenName == s; //s is a string variable</span></span>
* <span data-ttu-id="ec284-820">/ Objektmatris skapa uttryck - uttrycken returnerade ett objekt av sammansatta värde eller en anonym typ eller en matris med sådana objekt.</span><span class="sxs-lookup"><span data-stu-id="ec284-820">Object/array creation expression - these expressions return an object of compound value type or anonymous type or an array of such objects.</span></span> <span data-ttu-id="ec284-821">Dessa värden kan vara kapslade.</span><span class="sxs-lookup"><span data-stu-id="ec284-821">These values can be nested.</span></span>
  
     <span data-ttu-id="ec284-822">ny överordnad {familyName = ”Smith”, givenName = ”Johan”}; nya {först = 1, andra = 2}; en anonym typ med två fält</span><span class="sxs-lookup"><span data-stu-id="ec284-822">new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //an anonymous type with two fields</span></span>              
     <span data-ttu-id="ec284-823">nya int [] {3, child.grade, 5};</span><span class="sxs-lookup"><span data-stu-id="ec284-823">new int[] { 3, child.grade, 5 };</span></span>

### <span data-ttu-id="ec284-824"><a id="SupportedLinqOperators"></a>Lista över stöds LINQ-operatorer</span><span class="sxs-lookup"><span data-stu-id="ec284-824"><a id="SupportedLinqOperators"></a>List of supported LINQ operators</span></span>
<span data-ttu-id="ec284-825">Här är en lista över stöds LINQ operatorer i hello LINQ-providern ingår i hello .NET DocumentDB SDK.</span><span class="sxs-lookup"><span data-stu-id="ec284-825">Here is a list of supported LINQ operators in hello LINQ provider included with hello DocumentDB .NET SDK.</span></span>

* <span data-ttu-id="ec284-826">**Välj**: projektioner översätta toohello SQL väljer inklusive objektkonstruktion</span><span class="sxs-lookup"><span data-stu-id="ec284-826">**Select**: Projections translate toohello SQL SELECT including object construction</span></span>
* <span data-ttu-id="ec284-827">**Där**: filter översätta toohello SQL WHERE och stöd för översättningen mellan & &, || och!</span><span class="sxs-lookup"><span data-stu-id="ec284-827">**Where**: Filters translate toohello SQL WHERE, and support translation between && , || and !</span></span> <span data-ttu-id="ec284-828">toohello SQL-operatörer</span><span class="sxs-lookup"><span data-stu-id="ec284-828">toohello SQL operators</span></span>
* <span data-ttu-id="ec284-829">**SelectMany**: tillåter återgång av matriser toohello SQL JOIN-satsen.</span><span class="sxs-lookup"><span data-stu-id="ec284-829">**SelectMany**: Allows unwinding of arrays toohello SQL JOIN clause.</span></span> <span data-ttu-id="ec284-830">Kan använda toochain/kapsla uttryck toofilter på matriselement</span><span class="sxs-lookup"><span data-stu-id="ec284-830">Can be used toochain/nest expressions toofilter on array elements</span></span>
* <span data-ttu-id="ec284-831">**OrderBy och OrderByDescending**: översätter tooORDER BY stigande/fallande</span><span class="sxs-lookup"><span data-stu-id="ec284-831">**OrderBy and OrderByDescending**: Translates tooORDER BY ascending/descending</span></span>
* <span data-ttu-id="ec284-832">**Antal**, **summan**, **Min**, **Max**, och **genomsnittlig** operatorer för aggregering och motsvarigheterna asynkrona **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, och **AverageAsync**.</span><span class="sxs-lookup"><span data-stu-id="ec284-832">**Count**, **Sum**, **Min**, **Max**, and **Average** operators for aggregation, and their async equivalents **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, and **AverageAsync**.</span></span>
* <span data-ttu-id="ec284-833">**CompareTo**: översätter toorange jämförelser.</span><span class="sxs-lookup"><span data-stu-id="ec284-833">**CompareTo**: Translates toorange comparisons.</span></span> <span data-ttu-id="ec284-834">Används vanligtvis för strängar eftersom de inte jämförbar i .NET</span><span class="sxs-lookup"><span data-stu-id="ec284-834">Commonly used for strings since they’re not comparable in .NET</span></span>
* <span data-ttu-id="ec284-835">**Ta**: översätter toohello SQL upp för att begränsa resultaten av en fråga</span><span class="sxs-lookup"><span data-stu-id="ec284-835">**Take**: Translates toohello SQL TOP for limiting results from a query</span></span>
* <span data-ttu-id="ec284-836">**Matematiska funktioner**: har stöd för översättning av. NET'S Abs ARCCOS, ARCSIN ARCTAN, taket, Cos, Exp, våning, Log, Log10, Pow, runda, logga, Sin, Sqrt, Tan, trunkera toohello motsvarande SQL inbyggda funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-836">**Math Functions**: Supports translation from .NET’s Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="ec284-837">**Sträng funktioner**: har stöd för översättning av. NET'S Concat innehåller EndsWith, IndexOf, Count, ToLower, TrimStart, Ersätt, omvänd, TrimEnd, StartsWith, understrängen ToUpper toohello motsvarande SQL inbyggda funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-837">**String Functions**: Supports translation from .NET’s Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="ec284-838">**Matrisen funktioner**: har stöd för översättning av. NET'S Concat och innehåller antalet toohello motsvarande SQL inbyggda funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-838">**Array Functions**: Supports translation from .NET’s Concat, Contains, and Count toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="ec284-839">**Tilläggsfunktioner geospatiala**: har stöd för översättning av stub-metoder avståndet i IsValid, och IsValidDetailed toohello motsvarande SQL inbyggda funktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-839">**Geospatial Extension Functions**: Supports translation from stub methods Distance, Within, IsValid, and IsValidDetailed toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="ec284-840">**Användardefinierade funktionen tilläggsfunktionen**: har stöd för översättning av hello stub-metoden UserDefinedFunctionProvider.Invoke toohello motsvarande användardefinierad funktion.</span><span class="sxs-lookup"><span data-stu-id="ec284-840">**User-Defined Function Extension Function**: Supports translation from hello stub method UserDefinedFunctionProvider.Invoke toohello corresponding user-defined function.</span></span>
* <span data-ttu-id="ec284-841">**Diverse**: har stöd för översättning av hello slå samman och villkorlig operatörer.</span><span class="sxs-lookup"><span data-stu-id="ec284-841">**Miscellaneous**: Supports translation of hello coalesce and conditional operators.</span></span> <span data-ttu-id="ec284-842">Kan översätta innehåller tooString innehåller, ARRAY_CONTAINS eller hello SQL IN beroende på kontext.</span><span class="sxs-lookup"><span data-stu-id="ec284-842">Can translate Contains tooString CONTAINS, ARRAY_CONTAINS, or hello SQL IN depending on context.</span></span>

### <a name="sql-query-operators"></a><span data-ttu-id="ec284-843">SQL-frågeoperatorer</span><span class="sxs-lookup"><span data-stu-id="ec284-843">SQL query operators</span></span>
<span data-ttu-id="ec284-844">Här följer några exempel som visar hur vissa hello standard LINQ frågeoperatorer översätts ned tooCosmos DB frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-844">Here are some examples that illustrate how some of hello standard LINQ query operators are translated down tooCosmos DB queries.</span></span>

#### <a name="select-operator"></a><span data-ttu-id="ec284-845">Välj Operator</span><span class="sxs-lookup"><span data-stu-id="ec284-845">Select Operator</span></span>
<span data-ttu-id="ec284-846">hello-syntaxen är `input.Select(x => f(x))`, där `f` är ett skalärt uttryck.</span><span class="sxs-lookup"><span data-stu-id="ec284-846">hello syntax is `input.Select(x => f(x))`, where `f` is a scalar expression.</span></span>

<span data-ttu-id="ec284-847">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-847">**LINQ lambda expression**</span></span>

    input.Select(family => family.parents[0].familyName);

<span data-ttu-id="ec284-848">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-848">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



<span data-ttu-id="ec284-849">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-849">**LINQ lambda expression**</span></span>

    input.Select(family => family.children[0].grade + c); // c is an int variable


<span data-ttu-id="ec284-850">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-850">**SQL**</span></span> 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



<span data-ttu-id="ec284-851">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-851">**LINQ lambda expression**</span></span>

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


<span data-ttu-id="ec284-852">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-852">**SQL**</span></span> 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a><span data-ttu-id="ec284-853">SelectMany operator</span><span class="sxs-lookup"><span data-stu-id="ec284-853">SelectMany operator</span></span>
<span data-ttu-id="ec284-854">hello-syntaxen är `input.SelectMany(x => f(x))`, där `f` är ett skalärt uttryck som returnerar en Mängdtyp.</span><span class="sxs-lookup"><span data-stu-id="ec284-854">hello syntax is `input.SelectMany(x => f(x))`, where `f` is a scalar expression that returns a collection type.</span></span>

<span data-ttu-id="ec284-855">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-855">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children);

<span data-ttu-id="ec284-856">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-856">**SQL**</span></span> 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a><span data-ttu-id="ec284-857">Där operator</span><span class="sxs-lookup"><span data-stu-id="ec284-857">Where operator</span></span>
<span data-ttu-id="ec284-858">hello-syntaxen är `input.Where(x => f(x))`, där `f` är ett skalärt uttryck som returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="ec284-858">hello syntax is `input.Where(x => f(x))`, where `f` is a scalar expression, which returns a Boolean value.</span></span>

<span data-ttu-id="ec284-859">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-859">**LINQ lambda expression**</span></span>

    input.Where(family=> family.parents[0].familyName == "Smith");

<span data-ttu-id="ec284-860">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-860">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



<span data-ttu-id="ec284-861">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-861">**LINQ lambda expression**</span></span>

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

<span data-ttu-id="ec284-862">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-862">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a><span data-ttu-id="ec284-863">Sammansatta SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="ec284-863">Composite SQL queries</span></span>
<span data-ttu-id="ec284-864">hello ovan operatörer kan vara bestå tooform kraftfullare frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-864">hello above operators can be composed tooform more powerful queries.</span></span> <span data-ttu-id="ec284-865">Eftersom Cosmos DB stöder kapslade samlingar, kan hello sammansättning vara antingen sammanfogas eller kapslade.</span><span class="sxs-lookup"><span data-stu-id="ec284-865">Since Cosmos DB supports nested collections, hello composition can either be concatenated or nested.</span></span>

#### <a name="concatenation"></a><span data-ttu-id="ec284-866">Sammanfogning</span><span class="sxs-lookup"><span data-stu-id="ec284-866">Concatenation</span></span>
<span data-ttu-id="ec284-867">hello-syntaxen är `input(.|.SelectMany())(.Select()|.Where())*`.</span><span class="sxs-lookup"><span data-stu-id="ec284-867">hello syntax is `input(.|.SelectMany())(.Select()|.Where())*`.</span></span> <span data-ttu-id="ec284-868">En sammanfogad fråga kan börja med en valfri `SelectMany` frågan följt av flera `Select` eller `Where` operatörer.</span><span class="sxs-lookup"><span data-stu-id="ec284-868">A concatenated query can start with an optional `SelectMany` query followed by multiple `Select` or `Where` operators.</span></span>

<span data-ttu-id="ec284-869">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-869">**LINQ lambda expression**</span></span>

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

<span data-ttu-id="ec284-870">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-870">**SQL**</span></span>

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



<span data-ttu-id="ec284-871">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-871">**LINQ lambda expression**</span></span>

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

<span data-ttu-id="ec284-872">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-872">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



<span data-ttu-id="ec284-873">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-873">**LINQ lambda expression**</span></span>

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

<span data-ttu-id="ec284-874">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-874">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



<span data-ttu-id="ec284-875">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-875">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

<span data-ttu-id="ec284-876">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-876">**SQL**</span></span> 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a><span data-ttu-id="ec284-877">För många kapslade</span><span class="sxs-lookup"><span data-stu-id="ec284-877">Nesting</span></span>
<span data-ttu-id="ec284-878">hello-syntaxen är `input.SelectMany(x=>x.Q())` där Q är en `Select`, `SelectMany`, eller `Where` operator.</span><span class="sxs-lookup"><span data-stu-id="ec284-878">hello syntax is `input.SelectMany(x=>x.Q())` where Q is a `Select`, `SelectMany`, or `Where` operator.</span></span>

<span data-ttu-id="ec284-879">I en kapslad fråga är hello inre frågan tillämpade tooeach elementet för yttre mängden hello.</span><span class="sxs-lookup"><span data-stu-id="ec284-879">In a nested query, hello inner query is applied tooeach element of hello outer collection.</span></span> <span data-ttu-id="ec284-880">En viktig funktion är hello inre frågan kan referera toohello fält i hello element i hello yttre mängd som Självkopplingar.</span><span class="sxs-lookup"><span data-stu-id="ec284-880">One important feature is that hello inner query can refer toohello fields of hello elements in hello outer collection like self-joins.</span></span>

<span data-ttu-id="ec284-881">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-881">**LINQ lambda expression**</span></span>

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

<span data-ttu-id="ec284-882">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-882">**SQL**</span></span> 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


<span data-ttu-id="ec284-883">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-883">**LINQ lambda expression**</span></span>

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

<span data-ttu-id="ec284-884">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-884">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



<span data-ttu-id="ec284-885">**LINQ lambda-uttryck**</span><span class="sxs-lookup"><span data-stu-id="ec284-885">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

<span data-ttu-id="ec284-886">**SQL**</span><span class="sxs-lookup"><span data-stu-id="ec284-886">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <span data-ttu-id="ec284-887"><a id="ExecutingSqlQueries"></a>Kör SQL-frågor</span><span class="sxs-lookup"><span data-stu-id="ec284-887"><a id="ExecutingSqlQueries"></a>Executing SQL queries</span></span>
<span data-ttu-id="ec284-888">Cosmos DB visar resurser via ett REST-API som kan anropas med valfritt språk som kan göra HTTP/HTTPS-förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="ec284-888">Cosmos DB exposes resources through a REST API that can be called by any language capable of making HTTP/HTTPS requests.</span></span> <span data-ttu-id="ec284-889">Dessutom erbjuder Cosmos DB programmeringsbibliotek för flera populära språk som .NET, Node.js, JavaScript och Python.</span><span class="sxs-lookup"><span data-stu-id="ec284-889">Additionally, Cosmos DB offers programming libraries for several popular languages like .NET, Node.js, JavaScript, and Python.</span></span> <span data-ttu-id="ec284-890">hello REST-API och hello stöder olika bibliotek alla frågor via SQL.</span><span class="sxs-lookup"><span data-stu-id="ec284-890">hello REST API and hello various libraries all support querying through SQL.</span></span> <span data-ttu-id="ec284-891">hello .NET SDK stöder LINQ frågar dessutom tooSQL.</span><span class="sxs-lookup"><span data-stu-id="ec284-891">hello .NET SDK supports LINQ querying in addition tooSQL.</span></span>

<span data-ttu-id="ec284-892">Hej följande exempel visas hur toocreate en fråga och skicka den mot en Cosmos-DB-databaskonto.</span><span class="sxs-lookup"><span data-stu-id="ec284-892">hello following examples show how toocreate a query and submit it against a Cosmos DB database account.</span></span>

### <span data-ttu-id="ec284-893"><a id="RestAPI"></a>REST-API</span><span class="sxs-lookup"><span data-stu-id="ec284-893"><a id="RestAPI"></a>REST API</span></span>
<span data-ttu-id="ec284-894">Cosmos DB erbjuder en öppen RESTful-programmeringsmiljö via HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec284-894">Cosmos DB offers an open RESTful programming model over HTTP.</span></span> <span data-ttu-id="ec284-895">Databasen konton kan etableras med hjälp av en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ec284-895">Database accounts can be provisioned using an Azure subscription.</span></span> <span data-ttu-id="ec284-896">Hej Cosmos DB resursmodell består av en uppsättning resurser under ett databaskonto som är adresserbara via en logisk och stabil URI.</span><span class="sxs-lookup"><span data-stu-id="ec284-896">hello Cosmos DB resource model consists of a set of resources under a database account, each  of which is addressable using a logical and stable URI.</span></span> <span data-ttu-id="ec284-897">En uppsättning resurser är refererad tooas en feed i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ec284-897">A set of resources is referred tooas a feed in this document.</span></span> <span data-ttu-id="ec284-898">Ett databaskonto består av en uppsättning databaser som alla innehåller flera samlingar, var och en av vilka i sin tur innehåller dokument, UDF: er och andra typer av resurser.</span><span class="sxs-lookup"><span data-stu-id="ec284-898">A database account consists of a set of databases, each containing multiple collections, each of which in-turn contain documents, UDFs, and other resource types.</span></span>

<span data-ttu-id="ec284-899">hello grundläggande interaktion modellen med dessa resurser är via hello HTTP-verb GET, PUT, POST och DELETE med sina standard tolkning.</span><span class="sxs-lookup"><span data-stu-id="ec284-899">hello basic interaction model with these resources is through hello HTTP verbs GET, PUT, POST, and DELETE with their standard interpretation.</span></span> <span data-ttu-id="ec284-900">hello POST verb används för att skapa en ny resurs, för att köra en lagrad procedur eller för att utfärda en Cosmos-DB-fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-900">hello POST verb is used for creation of a new resource, for executing a stored procedure or for issuing a Cosmos DB query.</span></span> <span data-ttu-id="ec284-901">Frågor är alltid skrivskyddade åtgärder med inga sidoeffekter.</span><span class="sxs-lookup"><span data-stu-id="ec284-901">Queries are always read-only operations with no side-effects.</span></span>

<span data-ttu-id="ec284-902">hello visar följande exempel en POST för en DocumentDB-API-fråga som görs mot en samling som innehåller hello två exempeldokument vi har granskat hittills.</span><span class="sxs-lookup"><span data-stu-id="ec284-902">hello following examples show a POST for a DocumentDB API query made against a collection containing hello two sample documents we've reviewed so far.</span></span> <span data-ttu-id="ec284-903">hello-frågan har ett enkelt filter på hello JSON namnegenskapen.</span><span class="sxs-lookup"><span data-stu-id="ec284-903">hello query has a simple filter on hello JSON name property.</span></span> <span data-ttu-id="ec284-904">Observera hello användning av hello `x-ms-documentdb-isquery` och Content-Type: `application/query+json` huvuden toodenote som hello åtgärden är en fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-904">Note hello use of hello `x-ms-documentdb-isquery` and Content-Type: `application/query+json` headers toodenote that hello operation is a query.</span></span>

<span data-ttu-id="ec284-905">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="ec284-905">**Request**</span></span>

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


<span data-ttu-id="ec284-906">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-906">**Results**</span></span>

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


<span data-ttu-id="ec284-907">hello andra exemplet visar en mer komplex fråga som returnerar flera resultat från hello koppling.</span><span class="sxs-lookup"><span data-stu-id="ec284-907">hello second example shows a more complex query that returns multiple results from hello join.</span></span>

<span data-ttu-id="ec284-908">**Förfrågan**</span><span class="sxs-lookup"><span data-stu-id="ec284-908">**Request**</span></span>

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


<span data-ttu-id="ec284-909">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="ec284-909">**Results**</span></span>

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


<span data-ttu-id="ec284-910">Om en frågas resultat får inte plats i en enskild sida med resultat hello REST-API och returnerar sedan en fortsättningstoken via hello `x-ms-continuation-token` Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="ec284-910">If a query's results cannot fit within a single page of results, then hello REST API returns a continuation token through hello `x-ms-continuation-token` response header.</span></span> <span data-ttu-id="ec284-911">Klienter kan sidbryta resultat genom att inkludera hello-huvud i efterföljande resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-911">Clients can paginate results by including hello header in subsequent results.</span></span> <span data-ttu-id="ec284-912">hello antalet resultat per sida kan också kontrolleras via hello `x-ms-max-item-count` nummer sidhuvud.</span><span class="sxs-lookup"><span data-stu-id="ec284-912">hello number of results per page can also be controlled through hello `x-ms-max-item-count` number header.</span></span> <span data-ttu-id="ec284-913">Om hello angivna frågan har en Aggregeringsfunktion som `COUNT`, och sedan hello fråga sida kan returnera en delvis insamlat värde hello sida med resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-913">If hello specified query has an aggregation function like `COUNT`, then hello query page may return a partially aggregated value over hello page of results.</span></span> <span data-ttu-id="ec284-914">hello-klienter måste utföra en andra nivå sammanställning över dessa resultat tooproduce hello slutliga resultat, till exempel, sum över hello antal returneras i hello enskilda sidor tooreturn hello totala antalet.</span><span class="sxs-lookup"><span data-stu-id="ec284-914">hello clients must perform a second-level aggregation over these results tooproduce hello final results, for example, sum over hello counts returned in hello individual pages tooreturn hello total count.</span></span>

<span data-ttu-id="ec284-915">toomanage hello data konsekvent princip för frågor, Använd hello `x-ms-consistency-level` huvud som alla REST API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="ec284-915">toomanage hello data consistency policy for queries, use hello `x-ms-consistency-level` header like all REST API requests.</span></span> <span data-ttu-id="ec284-916">Sessionskonsekvens, är det obligatoriska tooalso echo hello senaste `x-ms-session-token` Cookie-huvud i hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="ec284-916">For session consistency, it is required tooalso echo hello latest `x-ms-session-token` Cookie header in hello query request.</span></span> <span data-ttu-id="ec284-917">hello kan efterfrågade samling indexprincip även påverka hello konsekvenskontroll av frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="ec284-917">hello queried collection's indexing policy can also influence hello consistency of query results.</span></span> <span data-ttu-id="ec284-918">Med hello standard indexering principinställningar för samlingar är hello index alltid aktuell med hello dokumentinnehåll och fråga resultatet överensstämmer hello konsekvenskontroll valt för data.</span><span class="sxs-lookup"><span data-stu-id="ec284-918">With hello default indexing policy settings, for collections hello index is always current with hello document contents and query results match hello consistency chosen for data.</span></span> <span data-ttu-id="ec284-919">Om hello indexering princip Avslappnad tooLazy kan frågor returnera inaktuella resultat.</span><span class="sxs-lookup"><span data-stu-id="ec284-919">If hello indexing policy is relaxed tooLazy, then queries can return stale results.</span></span> <span data-ttu-id="ec284-920">Mer information finns i [Azure Cosmos DB Konsekvensnivåer][consistency-levels].</span><span class="sxs-lookup"><span data-stu-id="ec284-920">For more information, see [Azure Cosmos DB Consistency Levels][consistency-levels].</span></span>

<span data-ttu-id="ec284-921">Om hello konfigurerad indexprincip på hello samling inte stöder hello angivna frågan, returnerar 400 ”Felaktig begäran” hello Azure Cosmos DB-server.</span><span class="sxs-lookup"><span data-stu-id="ec284-921">If hello configured indexing policy on hello collection cannot support hello specified query, hello Azure Cosmos DB server returns 400 "Bad Request".</span></span> <span data-ttu-id="ec284-922">Returneras för intervallet frågor mot sökvägar som konfigurerats för sökningar hash (likhetsfrågor) och för sökvägar som uttryckligen är undantagen från indexering.</span><span class="sxs-lookup"><span data-stu-id="ec284-922">This is returned for range queries against paths configured for hash (equality) lookups, and for paths explicitly excluded from indexing.</span></span> <span data-ttu-id="ec284-923">Hej `x-ms-documentdb-query-enable-scan` huvudet kan vara angivna tooallow hello frågan tooperform en genomsökningen när ett index inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="ec284-923">hello `x-ms-documentdb-query-enable-scan` header can be specified tooallow hello query tooperform a scan when an index is not available.</span></span>

<span data-ttu-id="ec284-924">Du kan få detaljerad mått på Frågekörningen genom att ange `x-ms-documentdb-populatequerymetrics` huvud för`True`.</span><span class="sxs-lookup"><span data-stu-id="ec284-924">You can get detailed metrics on query execution by setting `x-ms-documentdb-populatequerymetrics` header too`True`.</span></span> <span data-ttu-id="ec284-925">Mer information finns i [SQL-frågan mätvärden för Azure Cosmos DB DocumentDB API](documentdb-sql-query-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="ec284-925">For more information, see [SQL query metrics for Azure Cosmos DB DocumentDB API](documentdb-sql-query-metrics.md).</span></span>

### <span data-ttu-id="ec284-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span><span class="sxs-lookup"><span data-stu-id="ec284-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span></span>
<span data-ttu-id="ec284-927">hello .NET SDK stöder både LINQ och SQL frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-927">hello .NET SDK supports both LINQ and SQL querying.</span></span> <span data-ttu-id="ec284-928">hello följande exempel visas hur tooperform hello enkla filterfråga introduceras tidigare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ec284-928">hello following example shows how tooperform hello simple filter query introduced earlier in this document.</span></span>

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


<span data-ttu-id="ec284-929">Det här exemplet jämför två egenskaper för likhetsfrågor inom varje dokument och använder anonym projektioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-929">This sample compares two properties for equality within each document and uses anonymous projections.</span></span> 

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


<span data-ttu-id="ec284-930">hello nästa exempel visas kopplingar, uttryckt genom SelectMany för LINQ.</span><span class="sxs-lookup"><span data-stu-id="ec284-930">hello next sample shows joins, expressed through LINQ SelectMany.</span></span>

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



<span data-ttu-id="ec284-931">hello .NET klienten går automatiskt igenom alla hello sidor i frågeresultaten i hello foreach block som ovan.</span><span class="sxs-lookup"><span data-stu-id="ec284-931">hello .NET client automatically iterates through all hello pages of query results in hello foreach blocks as shown above.</span></span> <span data-ttu-id="ec284-932">hello frågan alternativ som introduceras i hello REST API-avsnittet är också tillgängliga i hello .NET SDK med hjälp av hello `FeedOptions` och `FeedResponse` klasser i hello CreateDocumentQuery metod.</span><span class="sxs-lookup"><span data-stu-id="ec284-932">hello query options introduced in hello REST API section are also available in hello .NET SDK using hello `FeedOptions` and `FeedResponse` classes in hello CreateDocumentQuery method.</span></span> <span data-ttu-id="ec284-933">hello antalet sidor som kan kontrolleras med hello `MaxItemCount` inställningen.</span><span class="sxs-lookup"><span data-stu-id="ec284-933">hello number of pages can be controlled using hello `MaxItemCount` setting.</span></span> 

<span data-ttu-id="ec284-934">Du kan också explicit styra sidindelning genom att skapa `IDocumentQueryable` med hello `IQueryable` objekt sedan genom att läsa den` ResponseContinuationToken` värden och skicka dem tillbaka som `RequestContinuationToken` i `FeedOptions`.</span><span class="sxs-lookup"><span data-stu-id="ec284-934">You can also explicitly control paging by creating `IDocumentQueryable` using hello `IQueryable` object, then by reading the` ResponseContinuationToken` values and passing them back as `RequestContinuationToken` in `FeedOptions`.</span></span> <span data-ttu-id="ec284-935">`EnableScanInQuery`kan vara set tooenable genomsökningar när hello frågan inte stöds av hello konfigurerade indexprincip.</span><span class="sxs-lookup"><span data-stu-id="ec284-935">`EnableScanInQuery` can be set tooenable scans when hello query cannot be supported by hello configured indexing policy.</span></span> <span data-ttu-id="ec284-936">För partitionerade samlingar kan du använda `PartitionKey` toorun hello frågan mot en enskild partition (även om Cosmos DB extraherar det automatiskt från hello frågetexten) och `EnableCrossPartitionQuery` toorun frågor som kan behöva toobe som körs mot flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-936">For partitioned collections, you can use `PartitionKey` toorun hello query against a single partition (though Cosmos DB can automatically extract this from hello query text), and `EnableCrossPartitionQuery` toorun queries that may need toobe run against multiple partitions.</span></span> 

<span data-ttu-id="ec284-937">Se för[Azure Cosmos DB .NET-exempel](https://github.com/Azure/azure-documentdb-net) för flera sampel som innehåller frågor.</span><span class="sxs-lookup"><span data-stu-id="ec284-937">Refer too[Azure Cosmos DB .NET samples](https://github.com/Azure/azure-documentdb-net) for more samples containing queries.</span></span> 

### <span data-ttu-id="ec284-938"><a id="JavaScriptServerSideApi"></a>JavaScript API för serversidan</span><span class="sxs-lookup"><span data-stu-id="ec284-938"><a id="JavaScriptServerSideApi"></a>JavaScript server-side API</span></span>
<span data-ttu-id="ec284-939">Cosmos DB är en programmeringsmodell för att köra JavaScript-baserade programlogik direkt på hello samlingar med lagrade procedurer och utlösare.</span><span class="sxs-lookup"><span data-stu-id="ec284-939">Cosmos DB provides a programming model for executing JavaScript based application logic directly on hello collections using stored procedures and triggers.</span></span> <span data-ttu-id="ec284-940">hello JavaScript-logik som registrerats på en samlingsnivå kan sedan utfärda databasåtgärder i hello åtgärder på hello dokument av hello angivna samlingen.</span><span class="sxs-lookup"><span data-stu-id="ec284-940">hello JavaScript logic registered at a collection level can then issue database operations on hello operations on hello documents of hello given collection.</span></span> <span data-ttu-id="ec284-941">Dessa åtgärder är kapslas in i omgivande ACID-transaktioner.</span><span class="sxs-lookup"><span data-stu-id="ec284-941">These operations are wrapped in ambient ACID transactions.</span></span>

<span data-ttu-id="ec284-942">hello följande exempel visas hur toouse hello Frågedokument i hello JavaScript server API toomake frågor från innanför lagrade procedurer och utlösare.</span><span class="sxs-lookup"><span data-stu-id="ec284-942">hello following example shows how toouse hello queryDocuments in hello JavaScript server API toomake queries from inside stored procedures and triggers.</span></span>

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

## <span data-ttu-id="ec284-943"><a id="References"></a>Referenser</span><span class="sxs-lookup"><span data-stu-id="ec284-943"><a id="References"></a>References</span></span>
1. <span data-ttu-id="ec284-944">[Introduktion tooAzure Cosmos DB][introduction]</span><span class="sxs-lookup"><span data-stu-id="ec284-944">[Introduction tooAzure Cosmos DB][introduction]</span></span>
2. [<span data-ttu-id="ec284-945">Azure Cosmos-Databasens SQL-specifikationen</span><span class="sxs-lookup"><span data-stu-id="ec284-945">Azure Cosmos DB SQL specification</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [<span data-ttu-id="ec284-946">Azure Cosmos DB .NET-exempel</span><span class="sxs-lookup"><span data-stu-id="ec284-946">Azure Cosmos DB .NET samples</span></span>](https://github.com/Azure/azure-documentdb-net)
4. <span data-ttu-id="ec284-947">[Konsekvensnivåer för Azure Cosmos DB][consistency-levels]</span><span class="sxs-lookup"><span data-stu-id="ec284-947">[Azure Cosmos DB Consistency Levels][consistency-levels]</span></span>
5. <span data-ttu-id="ec284-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span><span class="sxs-lookup"><span data-stu-id="ec284-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span></span>
6. <span data-ttu-id="ec284-949">JSON [http://json.org/](http://json.org/)</span><span class="sxs-lookup"><span data-stu-id="ec284-949">JSON [http://json.org/](http://json.org/)</span></span>
7. <span data-ttu-id="ec284-950">JavaScript-specifikationen [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span><span class="sxs-lookup"><span data-stu-id="ec284-950">Javascript Specification [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span></span> 
8. <span data-ttu-id="ec284-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span><span class="sxs-lookup"><span data-stu-id="ec284-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span></span> 
9. <span data-ttu-id="ec284-952">Fråga utvärdering tekniker för stora databaser [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span><span class="sxs-lookup"><span data-stu-id="ec284-952">Query evaluation techniques for large databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span></span>
10. <span data-ttu-id="ec284-953">Bearbetning av frågor i parallella relationsdatabassystem, IEEE datorn Society Press, 1994</span><span class="sxs-lookup"><span data-stu-id="ec284-953">Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994</span></span>
11. <span data-ttu-id="ec284-954">Lu Ooi, Tan, bearbetning av frågor i parallella relationsdatabassystem, IEEE datorn Society Press, 1994.</span><span class="sxs-lookup"><span data-stu-id="ec284-954">Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.</span></span>
12. <span data-ttu-id="ec284-955">Kitts Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Report Anders Tomkins: Pig Latin: en inte så främmande språk för databehandling, SIGMOD 2008.</span><span class="sxs-lookup"><span data-stu-id="ec284-955">Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.</span></span>
13. <span data-ttu-id="ec284-956">G.</span><span class="sxs-lookup"><span data-stu-id="ec284-956">G.</span></span> <span data-ttu-id="ec284-957">Graefe.</span><span class="sxs-lookup"><span data-stu-id="ec284-957">Graefe.</span></span> <span data-ttu-id="ec284-958">hello kaskadspridas ramverk för optimering av frågan.</span><span class="sxs-lookup"><span data-stu-id="ec284-958">hello Cascades framework for query optimization.</span></span> <span data-ttu-id="ec284-959">IEEE Data Eng.</span><span class="sxs-lookup"><span data-stu-id="ec284-959">IEEE Data Eng.</span></span> <span data-ttu-id="ec284-960">Bull., 18.3: 1995.</span><span class="sxs-lookup"><span data-stu-id="ec284-960">Bull., 18(3): 1995.</span></span>

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md