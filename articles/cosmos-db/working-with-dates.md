---
title: aaaWorking med datum i Azure Cosmos DB | Microsoft Docs
description: "Läs mer om hur toowork med datum i Azure Cosmos-databasen."
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
ms.openlocfilehash: 27ec170e4bef72c0b5b456738f1275ef02543024
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a><span data-ttu-id="8ca3e-103">Arbeta med datum i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8ca3e-103">Working with Dates in Azure Cosmos DB</span></span>
<span data-ttu-id="8ca3e-104">Azure Cosmos-DB ger schemaflexibilitet och omfattande indexering via ett ursprungligt [JSON](http://www.json.org) datamodellen.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-104">Azure Cosmos DB delivers schema flexibility and rich indexing via a native [JSON](http://www.json.org) data model.</span></span> <span data-ttu-id="8ca3e-105">Alla Azure Cosmos DB resurser inklusive databaser, samlingar, dokument och lagrade procedurer modelleras och lagras som JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-105">All Azure Cosmos DB resources including databases, collections, documents, and stored procedures are modeled and stored as JSON documents.</span></span> <span data-ttu-id="8ca3e-106">Som ett krav för att bärbara JSON (och Azure Cosmos DB) stöder bara en liten uppsättning grundläggande typer: sträng, Number, Boolean, matris, objekt och Null.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-106">As a requirement for being portable, JSON (and Azure Cosmos DB) supports only a small set of basic types: String, Number, Boolean, Array, Object, and Null.</span></span> <span data-ttu-id="8ca3e-107">JSON är flexibel och tillåta utvecklare och ramverk toorepresent mer komplexa typer med hjälp av dessa primitiver och skriva dem som objekt eller matriser.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-107">However, JSON is flexible and allow developers and frameworks toorepresent more complex types using these primitives and composing them as objects or arrays.</span></span> 

<span data-ttu-id="8ca3e-108">I tillägg toohello grundläggande typer många program behöver hello [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) skriva toorepresent datum och tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-108">In addition toohello basic types, many applications need hello [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type toorepresent dates and timestamps.</span></span> <span data-ttu-id="8ca3e-109">Den här artikeln beskriver hur utvecklare kan lagra, hämta och fråga datum i Azure Cosmos-databasen med hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-109">This article describes how developers can store, retrieve, and query dates in Azure Cosmos DB using hello .NET SDK.</span></span>

## <a name="storing-datetimes"></a><span data-ttu-id="8ca3e-110">Lagra datum och tid</span><span class="sxs-lookup"><span data-stu-id="8ca3e-110">Storing DateTimes</span></span>
<span data-ttu-id="8ca3e-111">Som standard hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) Serialiserar DateTime-värden som [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strängar.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-111">By default, hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializes DateTime values as [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strings.</span></span> <span data-ttu-id="8ca3e-112">De flesta program kan använda hello standard strängrepresentation för datum/tid för hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="8ca3e-112">Most applications can use hello default string representation for DateTime for hello following reasons:</span></span>

* <span data-ttu-id="8ca3e-113">Strängar kan jämföras och hello relativa sorteringen av hello DateTime-värden bevaras när de är transformerad toostrings.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-113">Strings can be compared, and hello relative ordering of hello DateTime values is preserved when they are transformed toostrings.</span></span> 
* <span data-ttu-id="8ca3e-114">Den här metoden kräver inte någon anpassad kod eller ett attribut för JSON-konvertering.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-114">This approach doesn't require any custom code or attributes for JSON conversion.</span></span>
* <span data-ttu-id="8ca3e-115">hello datum som lagras i JSON är mänsklig läsbar.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-115">hello dates as stored in JSON are human readable.</span></span>
* <span data-ttu-id="8ca3e-116">Den här metoden kan dra nytta av Azure Cosmos DB index för snabb frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-116">This approach can take advantage of Azure Cosmos DB's index for fast query performance.</span></span>

<span data-ttu-id="8ca3e-117">Till exempel hello följande fragment lagrar en `Order` objektegenskaper som innehåller två DateTime - `ShipDate` och `OrderDate` som ett dokument med hjälp av hello .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="8ca3e-117">For example, hello following snippet stores an `Order` object containing two DateTime properties - `ShipDate` and `OrderDate` as a document using hello .NET SDK:</span></span>

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

<span data-ttu-id="8ca3e-118">Det här dokumentet lagras i Azure Cosmos DB på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8ca3e-118">This document is stored in Azure Cosmos DB as follows:</span></span>

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

<span data-ttu-id="8ca3e-119">Du kan också lagra datum och tid som Unix tidsstämplar som är som ett tal som representerar hello antal förflutna sekunder sedan den 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-119">Alternatively, you can store DateTimes as Unix timestamps, that is, as a number representing hello number of elapsed seconds since January 1, 1970.</span></span> <span data-ttu-id="8ca3e-120">Azure Cosmos DB interna tidsstämpel (`_ts`) egenskapen följer den här metoden.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-120">Azure Cosmos DB's internal Timestamp (`_ts`) property follows this approach.</span></span> <span data-ttu-id="8ca3e-121">Du kan använda hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) klassen tooserialize datum och tid som tal.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-121">You can use hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) class tooserialize DateTimes as numbers.</span></span> 

## <a name="indexing-datetimes-for-range-queries"></a><span data-ttu-id="8ca3e-122">Indexering datum och tid för intervallet frågor</span><span class="sxs-lookup"><span data-stu-id="8ca3e-122">Indexing DateTimes for range queries</span></span>
<span data-ttu-id="8ca3e-123">Intervallet frågor är vanliga med DateTime-värden.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-123">Range queries are common with DateTime values.</span></span> <span data-ttu-id="8ca3e-124">Om du behöver toofind alla order som skapats sedan igår eller söka efter alla order som levererats i hello senaste fem minuterna, måste till exempel tooperform intervallet frågor.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-124">For example, if you need toofind all orders created since yesterday, or find all orders shipped in hello last five minutes, you need tooperform range queries.</span></span> <span data-ttu-id="8ca3e-125">tooexecute dessa frågor effektivt, måste du konfigurera din samling för intervallet indexering i strängar.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-125">tooexecute these queries efficiently, you must configure your collection for Range indexing on strings.</span></span>

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

<span data-ttu-id="8ca3e-126">Du kan lära dig mer om hur tooconfigure indexering principer på [Azure Cosmos DB indexering principer](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="8ca3e-126">You can learn more about how tooconfigure indexing policies at [Azure Cosmos DB Indexing Policies](indexing-policies.md).</span></span>

## <a name="querying-datetimes-in-linq"></a><span data-ttu-id="8ca3e-127">Frågar datum och tid i LINQ</span><span class="sxs-lookup"><span data-stu-id="8ca3e-127">Querying DateTimes in LINQ</span></span>
<span data-ttu-id="8ca3e-128">hello .NET DocumentDB SDK har automatiskt stöd för hämtning av data som lagras i Azure Cosmos DB via LINQ.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-128">hello DocumentDB .NET SDK automatically supports querying data stored in Azure Cosmos DB via LINQ.</span></span> <span data-ttu-id="8ca3e-129">Hello visar följande utdrag exempelvis en LINQ-fråga som filter order som levererades i hello senaste tre dagarna.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-129">For example, hello following snippet shows a LINQ query that filters orders that were shipped in hello last three days.</span></span>

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated toohello following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

<span data-ttu-id="8ca3e-130">Du kan lära dig mer om Azure Cosmos DB SQL-frågan språk och hello LINQ-providern på [frågar Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="8ca3e-130">You can learn more about Azure Cosmos DB's SQL query language and hello LINQ provider at [Querying Cosmos DB](documentdb-sql-query.md).</span></span>

<span data-ttu-id="8ca3e-131">I den här artikeln vi har tittat på hur toostore, index- och frågar efter datum och tid i Azure Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="8ca3e-131">In this article, we looked at how toostore, index, and query DateTimes in Azure Cosmos DB.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ca3e-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ca3e-132">Next Steps</span></span>
* <span data-ttu-id="8ca3e-133">Hämta och köra hello [kodexempel på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span><span class="sxs-lookup"><span data-stu-id="8ca3e-133">Download and run hello [Code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span></span>
* <span data-ttu-id="8ca3e-134">Lär dig mer om [DocumentDB API-fråga](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="8ca3e-134">Learn more about [DocumentDB API Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="8ca3e-135">Lär dig mer om [Azure Cosmos DB indexering principer](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="8ca3e-135">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>
