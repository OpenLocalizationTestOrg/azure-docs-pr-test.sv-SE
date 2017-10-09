---
title: aaaAzure SQL-databas JSON funktioner | Microsoft Docs
description: "Azure SQL-databas kan du tooparse, fråga och formatera data i JavaScript Object Notation (JSON) notation."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 55860105-2f5f-4b10-87a0-99faa32b5653
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.date: 11/15/2016
ms.author: jovanpop
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 30a31a1b01482ec276646b6fd6ca0c1f581168d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a><span data-ttu-id="d4140-103">Komma igång med JSON-funktioner i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="d4140-103">Getting started with JSON features in Azure SQL Database</span></span>
<span data-ttu-id="d4140-104">Azure SQL-databas, kan du analysera och frågar efter data som representeras i JavaScript Object Notation [(JSON)](http://www.json.org/) formatera och exportera din relationsdata som JSON-texten.</span><span class="sxs-lookup"><span data-stu-id="d4140-104">Azure SQL Database lets you parse and query data represented in JavaScript Object Notation [(JSON)](http://www.json.org/) format, and export your relational data as JSON text.</span></span>

<span data-ttu-id="d4140-105">JSON är ett populärt dataformat som används för att utbyta data i moderna webb- och mobilprogram.</span><span class="sxs-lookup"><span data-stu-id="d4140-105">JSON is a popular data format used for exchanging data in modern web and mobile applications.</span></span> <span data-ttu-id="d4140-106">JSON används också för att lagra halvstrukturerade data i loggfiler eller i NoSQL-databaser som [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="d4140-106">JSON is also used for storing semi-structured data in log files or in NoSQL databases like [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span></span> <span data-ttu-id="d4140-107">Många REST-webbtjänsterna sökresultaten formateras som JSON-text eller acceptera data formateras som JSON.</span><span class="sxs-lookup"><span data-stu-id="d4140-107">Many REST web services return results formatted as JSON text or accept data formatted as JSON.</span></span> <span data-ttu-id="d4140-108">De flesta Azure services som [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), och [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) har REST-slutpunkter som returnerar eller använda JSON.</span><span class="sxs-lookup"><span data-stu-id="d4140-108">Most Azure services such as [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), and [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) have REST endpoints that return or consume JSON.</span></span>

<span data-ttu-id="d4140-109">Azure SQL-databas kan du enkelt arbeta med JSON-data och integrera din databas med moderna tjänster.</span><span class="sxs-lookup"><span data-stu-id="d4140-109">Azure SQL Database lets you work with JSON data easily and integrate your database with modern services.</span></span>

## <a name="overview"></a><span data-ttu-id="d4140-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="d4140-110">Overview</span></span>
<span data-ttu-id="d4140-111">Azure SQL Database ger hello följande funktioner för att arbeta med JSON-data:</span><span class="sxs-lookup"><span data-stu-id="d4140-111">Azure SQL Database provides hello following functions for working with JSON data:</span></span>

![JSON-funktioner](./media/sql-database-json-features/image_1.png)

<span data-ttu-id="d4140-113">Om du har JSON-text, du kan extrahera data från JSON eller kontrollera att JSON korrekt formaterad med inbyggda funktioner för hello [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), och [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx) .</span><span class="sxs-lookup"><span data-stu-id="d4140-113">If you have JSON text, you can extract data from JSON or verify that JSON is properly formatted by using hello built-in functions [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), and [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx).</span></span> <span data-ttu-id="d4140-114">Hej [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) funktionen kan du uppdatera värdet i JSON-texten.</span><span class="sxs-lookup"><span data-stu-id="d4140-114">hello [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) function lets you update value inside JSON text.</span></span> <span data-ttu-id="d4140-115">Mer avancerade frågor och analys, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) funktion kan omvandla en matris av JSON-objekt till en uppsättning rader.</span><span class="sxs-lookup"><span data-stu-id="d4140-115">For more advanced querying and analysis, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) function can transform an array of JSON objects into a set of rows.</span></span> <span data-ttu-id="d4140-116">SQL-frågan kan köras på hello returnerade en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="d4140-116">Any SQL query can be executed on hello returned result set.</span></span> <span data-ttu-id="d4140-117">Slutligen är det en [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) -sats som gör att du kan formatera data som lagras i relationella tabellerna som JSON-text.</span><span class="sxs-lookup"><span data-stu-id="d4140-117">Finally, there is a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) clause that lets you format data stored in your relational tables as JSON text.</span></span>

## <a name="formatting-relational-data-in-json-format"></a><span data-ttu-id="d4140-118">Formatera relationella data i JSON-format</span><span class="sxs-lookup"><span data-stu-id="d4140-118">Formatting relational data in JSON format</span></span>
<span data-ttu-id="d4140-119">Om du har en webbtjänst som tar data från databasen hello lager och ger ett svar i JSON-format eller klientens JavaScript-ramverk och bibliotek som accepterar data formateras som JSON kan formatera du innehållet i databasen som JSON direkt i en SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="d4140-119">If you have a web service that takes data from hello database layer and provides a response in JSON format, or client-side JavaScript frameworks or libraries that accept data formatted as JSON, you can format your database content as JSON directly in a SQL query.</span></span> <span data-ttu-id="d4140-120">Du inte längre har toowrite programkod som formaterar resultat från Azure SQL Database som JSON, eller innehåller vissa JSON-serialisering biblioteket tooconvert tabellfråga resultat och serialisera objekt tooJSON format.</span><span class="sxs-lookup"><span data-stu-id="d4140-120">You no longer have toowrite application code that formats results from Azure SQL Database as JSON, or include some JSON serialization library tooconvert tabular query results and then serialize objects tooJSON format.</span></span> <span data-ttu-id="d4140-121">Du kan i stället använda hello för JSON-sats tooformat SQL frågeresultat som JSON i Azure SQL Database och använda det direkt i ditt program.</span><span class="sxs-lookup"><span data-stu-id="d4140-121">Instead, you can use hello FOR JSON clause tooformat SQL query results as JSON in Azure SQL Database and use it directly in your application.</span></span>

<span data-ttu-id="d4140-122">I följande exempel hello, formateras rader från hello Sales.Customer tabellen som JSON med hello FOR JSON-sats:</span><span class="sxs-lookup"><span data-stu-id="d4140-122">In hello following example, rows from hello Sales.Customer table are formatted as JSON by using hello FOR JSON clause:</span></span>

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

<span data-ttu-id="d4140-123">hello FOR JSON PATH satsen formaterar hello resultaten av hello-frågan som JSON-texten.</span><span class="sxs-lookup"><span data-stu-id="d4140-123">hello FOR JSON PATH clause formats hello results of hello query as JSON text.</span></span> <span data-ttu-id="d4140-124">Kolumnnamn används som nycklar, även om hello cellvärden genereras som JSON-värden:</span><span class="sxs-lookup"><span data-stu-id="d4140-124">Column names are used as keys, while hello cell values are generated as JSON values:</span></span>

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

<span data-ttu-id="d4140-125">hello resultatmängden formateras som en JSON-matris där varje rad formateras som en separat JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="d4140-125">hello result set is formatted as a JSON array where each row is formatted as a separate JSON object.</span></span>

<span data-ttu-id="d4140-126">SÖKVÄGEN anger att du kan anpassa hello utdataformatet för JSON-resultat med hjälp av punktnotation i kolumnalias.</span><span class="sxs-lookup"><span data-stu-id="d4140-126">PATH indicates that you can customize hello output format of your JSON result by using dot notation in column aliases.</span></span> <span data-ttu-id="d4140-127">följande fråga hello ändras hello namnet på hello ”CustomerName” nyckel i hello utdata JSON-format och placerar telefon-och faxnummer i hello ”kontakta” underordnade objekt:</span><span class="sxs-lookup"><span data-stu-id="d4140-127">hello following query changes hello name of hello "CustomerName" key in hello output JSON format, and puts phone and fax numbers in hello "Contact" sub-object:</span></span>

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

<span data-ttu-id="d4140-128">hello utdata från den här frågan ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="d4140-128">hello output of this query looks like this:</span></span>

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

<span data-ttu-id="d4140-129">I det här exemplet returneras vi ett JSON-objekt i stället för en matris genom att ange hello [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) alternativet.</span><span class="sxs-lookup"><span data-stu-id="d4140-129">In this example we returned a single JSON object instead of an array by specifying hello [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) option.</span></span> <span data-ttu-id="d4140-130">Du kan använda det här alternativet om du vet att du returnerar ett objekt på grund av frågan.</span><span class="sxs-lookup"><span data-stu-id="d4140-130">You can use this option if you know that you are returning a single object as a result of query.</span></span>

<span data-ttu-id="d4140-131">hello huvudsakliga värdet för hello FOR JSON-sats är att du kan returnera komplexa hierarkiska data från databasen formaterade som kapslade JSON-objekt eller matriser.</span><span class="sxs-lookup"><span data-stu-id="d4140-131">hello main value of hello FOR JSON clause is that it lets you return complex hierarchical data from your database formatted as nested JSON objects or arrays.</span></span> <span data-ttu-id="d4140-132">följande exempel visar hur tooinclude order som tillhör toohello kund som en kapslad order-matris hello:</span><span class="sxs-lookup"><span data-stu-id="d4140-132">hello following example shows how tooinclude Orders that belong toohello Customer as a nested array of Orders:</span></span>

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

<span data-ttu-id="d4140-133">I stället för att skicka separata frågor tooget kunddata och toofetch en lista över relaterade order, kan du alla nödvändiga hello-data med en enskild fråga som visas i följande exempel på utdata hello:</span><span class="sxs-lookup"><span data-stu-id="d4140-133">Instead of sending separate queries tooget Customer data and then toofetch a list of related Orders, you can get all hello necessary data with a single query, as shown in hello following sample output:</span></span>

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a><span data-ttu-id="d4140-134">Arbeta med JSON-data</span><span class="sxs-lookup"><span data-stu-id="d4140-134">Working with JSON data</span></span>
<span data-ttu-id="d4140-135">Om du inte har strikt strukturerade data om du har komplexa underordnade objekt, matriser eller hierarkiska data eller om din datastrukturer utvecklas över tid, hello JSON-format kan hjälpa dig att toorepresent någon komplexa data-struktur.</span><span class="sxs-lookup"><span data-stu-id="d4140-135">If you don’t have strictly structured data, if you have complex sub-objects, arrays, or hierarchical data, or if your data structures evolve over time, hello JSON format can help you toorepresent any complex data structure.</span></span>

<span data-ttu-id="d4140-136">JSON är en textrepresentation format som kan användas som en annan strängtyp i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d4140-136">JSON is a textual format that can be used like any other string type in Azure SQL Database.</span></span> <span data-ttu-id="d4140-137">Du kan skicka eller lagra JSON-data som standard NVARCHAR:</span><span class="sxs-lookup"><span data-stu-id="d4140-137">You can send or store JSON data as a standard NVARCHAR:</span></span>

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

<span data-ttu-id="d4140-138">hello JSON-data som används i det här exemplet representeras med hjälp av hello NVARCHAR(MAX).</span><span class="sxs-lookup"><span data-stu-id="d4140-138">hello JSON data used in this example is represented by using hello NVARCHAR(MAX) type.</span></span> <span data-ttu-id="d4140-139">JSON kan infogas i den här tabellen eller tillhandahålls som ett argument av hello lagrade proceduren med standard Transact-SQL-syntax som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="d4140-139">JSON can be inserted into this table or provided as an argument of hello stored procedure using standard Transact-SQL syntax as shown in hello following example:</span></span>

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

<span data-ttu-id="d4140-140">Alla språk för klientsidan eller som fungerar med strängdata i Azure SQL Database-biblioteket fungerar även med JSON-data.</span><span class="sxs-lookup"><span data-stu-id="d4140-140">Any client-side language or library that works with string data in Azure SQL Database will also work with JSON data.</span></span> <span data-ttu-id="d4140-141">JSON kan lagras i en tabell som har stöd för hello NVARCHAR typ, till exempel en minnesoptimerad tabell eller en systemversionstabell.</span><span class="sxs-lookup"><span data-stu-id="d4140-141">JSON can be stored in any table that supports hello NVARCHAR type, such as a Memory-optimized table or a System-versioned table.</span></span> <span data-ttu-id="d4140-142">JSON införs inte någon begränsning i hello klientkod eller i hello databasen lager.</span><span class="sxs-lookup"><span data-stu-id="d4140-142">JSON does not introduce any constraint either in hello client-side code or in hello database layer.</span></span>

## <a name="querying-json-data"></a><span data-ttu-id="d4140-143">Hämtning av JSON-data</span><span class="sxs-lookup"><span data-stu-id="d4140-143">Querying JSON data</span></span>
<span data-ttu-id="d4140-144">Om du har data som är formaterade som JSON som lagras i Azure SQL-tabeller, kan du använda dessa data i SQL-frågan i JSON-funktioner.</span><span class="sxs-lookup"><span data-stu-id="d4140-144">If you have data formatted as JSON stored in Azure SQL tables, JSON functions let you use this data in any SQL query.</span></span>

<span data-ttu-id="d4140-145">JSON-funktioner som är tillgängliga i Azure SQL-databasen kan du hantera data som är formaterade som JSON som alla andra SQL-datatyper.</span><span class="sxs-lookup"><span data-stu-id="d4140-145">JSON functions that are available in Azure SQL database let you treat data formatted as JSON as any other SQL data type.</span></span> <span data-ttu-id="d4140-146">Du kan enkelt extrahera värden från hello JSON-text och använda JSON-data i en fråga:</span><span class="sxs-lookup"><span data-stu-id="d4140-146">You can easily extract values from hello JSON text, and use JSON data in any query:</span></span>

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

<span data-ttu-id="d4140-147">Hej JSON_VALUE funktionen extraherar ett värde från JSON-texten som lagras i hello datakolumn.</span><span class="sxs-lookup"><span data-stu-id="d4140-147">hello JSON_VALUE function extracts a value from JSON text stored in hello Data column.</span></span> <span data-ttu-id="d4140-148">Den här funktionen använder en JavaScript-liknande sökvägen tooreference ett värde i JSON-texten tooextract.</span><span class="sxs-lookup"><span data-stu-id="d4140-148">This function uses a JavaScript-like path tooreference a value in JSON text tooextract.</span></span> <span data-ttu-id="d4140-149">hello extraherade värdet kan användas i valfri del av SQL-frågan.</span><span class="sxs-lookup"><span data-stu-id="d4140-149">hello extracted value can be used in any part of SQL query.</span></span>

<span data-ttu-id="d4140-150">Hej JSON_QUERY funktion är liknande tooJSON_VALUE.</span><span class="sxs-lookup"><span data-stu-id="d4140-150">hello JSON_QUERY function is similar tooJSON_VALUE.</span></span> <span data-ttu-id="d4140-151">Till skillnad från JSON_VALUE extraherar komplexa underordnade objekt, till exempel matriser eller objekt som är placerade i JSON-texten i den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d4140-151">Unlike JSON_VALUE, this function extracts complex sub-object such as arrays or objects that are placed in JSON text.</span></span>

<span data-ttu-id="d4140-152">Hej JSON_MODIFY funktionen kan du ange hello sökvägen hello värdet i hello JSON-texten som ska uppdateras, samt ett nytt värde som skrivs hello gamla.</span><span class="sxs-lookup"><span data-stu-id="d4140-152">hello JSON_MODIFY function lets you specify hello path of hello value in hello JSON text that should be updated, as well as a new value that will overwrite hello old one.</span></span> <span data-ttu-id="d4140-153">Det här sättet kan du enkelt uppdatera JSON-texten utan reparsing hello hela strukturen.</span><span class="sxs-lookup"><span data-stu-id="d4140-153">This way you can easily update JSON text without reparsing hello entire structure.</span></span>

<span data-ttu-id="d4140-154">Eftersom JSON finns i vanlig text, finns det inga garantier att hello värden som lagras i textkolumner korrekt formaterad.</span><span class="sxs-lookup"><span data-stu-id="d4140-154">Since JSON is stored in a standard text, there are no guarantees that hello values stored in text columns are properly formatted.</span></span> <span data-ttu-id="d4140-155">Du kan kontrollera att texten som lagras i JSON-kolumn har utformats korrekt genom att använda standard Kontrollbegränsningar för Azure SQL Database och hello ISJSON funktionen:</span><span class="sxs-lookup"><span data-stu-id="d4140-155">You can verify that text stored in JSON column is properly formatted by using standard Azure SQL Database check constraints and hello ISJSON function:</span></span>

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

<span data-ttu-id="d4140-156">Om hello-indata är korrekt formaterad JSON, hello ISJSON funktionen returnerar hello värdet 1.</span><span class="sxs-lookup"><span data-stu-id="d4140-156">If hello input text is properly formatted JSON, hello ISJSON function returns hello value 1.</span></span> <span data-ttu-id="d4140-157">Det här villkoret kommer på varje insert eller update för JSON-kolumnen Kontrollera att nya textvärdet inte är felaktiga JSON.</span><span class="sxs-lookup"><span data-stu-id="d4140-157">On every insert or update of JSON column, this constraint will verify that new text value is not malformed JSON.</span></span>

## <a name="transforming-json-into-tabular-format"></a><span data-ttu-id="d4140-158">Omvandla JSON i tabellformat</span><span class="sxs-lookup"><span data-stu-id="d4140-158">Transforming JSON into tabular format</span></span>
<span data-ttu-id="d4140-159">Azure SQL-databas kan du omvandla JSON-samlingar i tabelldata för JSON-format och Läs in eller frågan.</span><span class="sxs-lookup"><span data-stu-id="d4140-159">Azure SQL Database also lets you transform JSON collections into tabular format and load or query JSON data.</span></span>

<span data-ttu-id="d4140-160">OPENJSON är en funktion i tabell-värde som Parsar JSON-text, söker efter en matris av JSON-objekt, går igenom hello elementen i hello matrisen och returnerar en rad i hello utgående resultatet för varje element i matrisen hello.</span><span class="sxs-lookup"><span data-stu-id="d4140-160">OPENJSON is a table-value function that parses JSON text, locates an array of JSON objects, iterates through hello elements of hello array, and returns one row in hello output result for each element of hello array.</span></span>

![JSON tabular](./media/sql-database-json-features/image_2.png)

<span data-ttu-id="d4140-162">I hello-exemplet ovan, kan vi ange där toolocate hello JSON-matris som ska öppnas (i hello $. Order sökväg), vilka kolumner som ska returneras som resultat och där toofind hello JSON-värden som ska returneras som celler.</span><span class="sxs-lookup"><span data-stu-id="d4140-162">In hello example above, we can specify where toolocate hello JSON array that should be opened (in hello $.Orders path), what columns should be returned as result, and where toofind hello JSON values that will be returned as cells.</span></span>

<span data-ttu-id="d4140-163">Vi kan omvandla en JSON-matris på hello @orders variabel till en uppsättning rader, analysera resultatuppsättningen eller infoga rader i en tabell som standard:</span><span class="sxs-lookup"><span data-stu-id="d4140-163">We can transform a JSON array in hello @orders variable into a set of rows, analyze this result set, or insert rows into a standard table:</span></span>

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
<span data-ttu-id="d4140-164">hello samling order formateras som en JSON-matris och tillhandahållas som en parameter toohello lagrad procedur kan parsas och infogas i tabellen för hello order.</span><span class="sxs-lookup"><span data-stu-id="d4140-164">hello collection of orders formatted as a JSON array and provided as a parameter toohello stored procedure can be parsed and inserted into hello Orders table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4140-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4140-165">Next steps</span></span>
<span data-ttu-id="d4140-166">toolearn hur toointegrate JSON till programmet, titta närmare på dessa resurser:</span><span class="sxs-lookup"><span data-stu-id="d4140-166">toolearn how toointegrate JSON into your application, check out these resources:</span></span>

* [<span data-ttu-id="d4140-167">TechNet-blogg</span><span class="sxs-lookup"><span data-stu-id="d4140-167">TechNet Blog</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [<span data-ttu-id="d4140-168">MSDN-dokumentationen</span><span class="sxs-lookup"><span data-stu-id="d4140-168">MSDN documentation</span></span>](https://msdn.microsoft.com/library/dn921897.aspx)
* [<span data-ttu-id="d4140-169">Channel 9 video</span><span class="sxs-lookup"><span data-stu-id="d4140-169">Channel 9 video</span></span>](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

<span data-ttu-id="d4140-170">toolearn om olika scenarier för att integrera JSON i ditt program, se hello demonstrationer i det här [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) eller söka efter ett scenario som matchar dina användningsfall i [JSON blogginlägg](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span><span class="sxs-lookup"><span data-stu-id="d4140-170">toolearn about various scenarios for integrating JSON into your application, see hello demos in this [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) or find a scenario that matches your use case in [JSON Blog posts](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span></span>

