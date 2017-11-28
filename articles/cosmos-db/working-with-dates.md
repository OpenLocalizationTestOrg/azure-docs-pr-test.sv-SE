---
title: Arbeta med datum i Azure Cosmos DB | Microsoft Docs
description: "Läs mer om hur du arbetar med datum i Azure Cosmos DB."
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: e587772f-ce9f-498c-a017-a51e7265bb23
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: b6a77e33eea24000037ffb31d7aae3cb1d345ce9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a><span data-ttu-id="ec840-103">Arbeta med datum i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="ec840-103">Working with Dates in Azure Cosmos DB</span></span>
<span data-ttu-id="ec840-104">Azure Cosmos-DB ger schemaflexibilitet och omfattande indexering via ett ursprungligt [JSON](http://www.json.org) datamodellen.</span><span class="sxs-lookup"><span data-stu-id="ec840-104">Azure Cosmos DB delivers schema flexibility and rich indexing via a native [JSON](http://www.json.org) data model.</span></span> <span data-ttu-id="ec840-105">Alla Azure Cosmos DB resurser inklusive databaser, samlingar, dokument och lagrade procedurer modelleras och lagras som JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="ec840-105">All Azure Cosmos DB resources including databases, collections, documents, and stored procedures are modeled and stored as JSON documents.</span></span> <span data-ttu-id="ec840-106">Som ett krav för att bärbara JSON (och Azure Cosmos DB) stöder bara en liten uppsättning grundläggande typer: sträng, Number, Boolean, matris, objekt och Null.</span><span class="sxs-lookup"><span data-stu-id="ec840-106">As a requirement for being portable, JSON (and Azure Cosmos DB) supports only a small set of basic types: String, Number, Boolean, Array, Object, and Null.</span></span> <span data-ttu-id="ec840-107">JSON är flexibel och kan utvecklare och ramverk att representera mer komplexa typer med hjälp av dessa primitiver och skriva dem som objekt eller matriser.</span><span class="sxs-lookup"><span data-stu-id="ec840-107">However, JSON is flexible and allow developers and frameworks to represent more complex types using these primitives and composing them as objects or arrays.</span></span> 

<span data-ttu-id="ec840-108">Utöver de grundläggande typerna många program behöver den [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) typ som representerar datum och tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="ec840-108">In addition to the basic types, many applications need the [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type to represent dates and timestamps.</span></span> <span data-ttu-id="ec840-109">Den här artikeln beskriver hur utvecklare kan lagra, hämta och fråga datum i Azure Cosmos-databasen med .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="ec840-109">This article describes how developers can store, retrieve, and query dates in Azure Cosmos DB using the .NET SDK.</span></span>

## <a name="storing-datetimes"></a><span data-ttu-id="ec840-110">Lagra datum och tid</span><span class="sxs-lookup"><span data-stu-id="ec840-110">Storing DateTimes</span></span>
<span data-ttu-id="ec840-111">Som standard den [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) Serialiserar DateTime-värden som [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strängar.</span><span class="sxs-lookup"><span data-stu-id="ec840-111">By default, the [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializes DateTime values as [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strings.</span></span> <span data-ttu-id="ec840-112">De flesta program kan använda standard strängrepresentation för datum/tid av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="ec840-112">Most applications can use the default string representation for DateTime for the following reasons:</span></span>

* <span data-ttu-id="ec840-113">Strängar kan jämföras och DateTime-värden relativa ordning bevaras när de omvandlas till strängar.</span><span class="sxs-lookup"><span data-stu-id="ec840-113">Strings can be compared, and the relative ordering of the DateTime values is preserved when they are transformed to strings.</span></span> 
* <span data-ttu-id="ec840-114">Den här metoden kräver inte någon anpassad kod eller ett attribut för JSON-konvertering.</span><span class="sxs-lookup"><span data-stu-id="ec840-114">This approach doesn't require any custom code or attributes for JSON conversion.</span></span>
* <span data-ttu-id="ec840-115">Datum som lagras i JSON är mänsklig läsbar.</span><span class="sxs-lookup"><span data-stu-id="ec840-115">The dates as stored in JSON are human readable.</span></span>
* <span data-ttu-id="ec840-116">Den här metoden kan dra nytta av Azure Cosmos DB index för snabb frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="ec840-116">This approach can take advantage of Azure Cosmos DB's index for fast query performance.</span></span>

<span data-ttu-id="ec840-117">Till exempel följande kodavsnitt lagrar en `Order` objektegenskaper som innehåller två DateTime - `ShipDate` och `OrderDate` som ett dokument med .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="ec840-117">For example, the following snippet stores an `Order` object containing two DateTime properties - `ShipDate` and `OrderDate` as a document using the .NET SDK:</span></span>

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

<span data-ttu-id="ec840-118">Det här dokumentet lagras i Azure Cosmos DB på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ec840-118">This document is stored in Azure Cosmos DB as follows:</span></span>

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

<span data-ttu-id="ec840-119">Du kan också lagra datum och tid som Unix tidsstämplar som är som ett tal som representerar antalet förflutna sekunder sedan den 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="ec840-119">Alternatively, you can store DateTimes as Unix timestamps, that is, as a number representing the number of elapsed seconds since January 1, 1970.</span></span> <span data-ttu-id="ec840-120">Azure Cosmos DB interna tidsstämpel (`_ts`) egenskapen följer den här metoden.</span><span class="sxs-lookup"><span data-stu-id="ec840-120">Azure Cosmos DB's internal Timestamp (`_ts`) property follows this approach.</span></span> <span data-ttu-id="ec840-121">Du kan använda den [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) klassen för att serialisera datum och tid som tal.</span><span class="sxs-lookup"><span data-stu-id="ec840-121">You can use the [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) class to serialize DateTimes as numbers.</span></span> 

## <a name="indexing-datetimes-for-range-queries"></a><span data-ttu-id="ec840-122">Indexering datum och tid för intervallet frågor</span><span class="sxs-lookup"><span data-stu-id="ec840-122">Indexing DateTimes for range queries</span></span>
<span data-ttu-id="ec840-123">Intervallet frågor är vanliga med DateTime-värden.</span><span class="sxs-lookup"><span data-stu-id="ec840-123">Range queries are common with DateTime values.</span></span> <span data-ttu-id="ec840-124">Om du behöver hitta alla order som skapats sedan igår, eller söka efter alla order som levererats under de senaste fem minuterna, måste du utföra intervallet frågor.</span><span class="sxs-lookup"><span data-stu-id="ec840-124">For example, if you need to find all orders created since yesterday, or find all orders shipped in the last five minutes, you need to perform range queries.</span></span> <span data-ttu-id="ec840-125">Om du vill köra dessa frågor effektivt måste du konfigurera din samling för intervallet indexering i strängar.</span><span class="sxs-lookup"><span data-stu-id="ec840-125">To execute these queries efficiently, you must configure your collection for Range indexing on strings.</span></span>

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

<span data-ttu-id="ec840-126">Du kan lära dig mer om hur du konfigurerar indexering principer på [Azure Cosmos DB indexering principer](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="ec840-126">You can learn more about how to configure indexing policies at [Azure Cosmos DB Indexing Policies](indexing-policies.md).</span></span>

## <a name="querying-datetimes-in-linq"></a><span data-ttu-id="ec840-127">Frågar datum och tid i LINQ</span><span class="sxs-lookup"><span data-stu-id="ec840-127">Querying DateTimes in LINQ</span></span>
<span data-ttu-id="ec840-128">.NET DocumentDB SDK har automatiskt stöd för hämtning av data som lagras i Azure Cosmos DB via LINQ.</span><span class="sxs-lookup"><span data-stu-id="ec840-128">The DocumentDB .NET SDK automatically supports querying data stored in Azure Cosmos DB via LINQ.</span></span> <span data-ttu-id="ec840-129">Följande utdrag visar exempelvis en LINQ-fråga som filter order som levererades under de senaste tre dagarna.</span><span class="sxs-lookup"><span data-stu-id="ec840-129">For example, the following snippet shows a LINQ query that filters orders that were shipped in the last three days.</span></span>

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

<span data-ttu-id="ec840-130">Du kan lära dig mer om Azure Cosmos DB SQL-frågespråket och LINQ-providern på [frågar Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="ec840-130">You can learn more about Azure Cosmos DB's SQL query language and the LINQ provider at [Querying Cosmos DB](documentdb-sql-query.md).</span></span>

<span data-ttu-id="ec840-131">I den här artikeln vi har tittat på hur du lagrar, index- och fråga datum och tid i Azure Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="ec840-131">In this article, we looked at how to store, index, and query DateTimes in Azure Cosmos DB.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec840-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec840-132">Next Steps</span></span>
* <span data-ttu-id="ec840-133">Hämta och kör den [kodexempel på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span><span class="sxs-lookup"><span data-stu-id="ec840-133">Download and run the [Code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span></span>
* <span data-ttu-id="ec840-134">Lär dig mer om [DocumentDB API-fråga](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="ec840-134">Learn more about [DocumentDB API Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="ec840-135">Lär dig mer om [Azure Cosmos DB indexering principer](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="ec840-135">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>
