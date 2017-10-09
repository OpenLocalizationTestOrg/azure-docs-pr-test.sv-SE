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
# <a name="working-with-dates-in-azure-cosmos-db"></a>Arbeta med datum i Azure Cosmos DB
Azure Cosmos-DB ger schemaflexibilitet och omfattande indexering via ett ursprungligt [JSON](http://www.json.org) datamodellen. Alla Azure Cosmos DB resurser inklusive databaser, samlingar, dokument och lagrade procedurer modelleras och lagras som JSON-dokument. Som ett krav för att bärbara JSON (och Azure Cosmos DB) stöder bara en liten uppsättning grundläggande typer: sträng, Number, Boolean, matris, objekt och Null. JSON är flexibel och tillåta utvecklare och ramverk toorepresent mer komplexa typer med hjälp av dessa primitiver och skriva dem som objekt eller matriser. 

I tillägg toohello grundläggande typer många program behöver hello [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) skriva toorepresent datum och tidsstämplar. Den här artikeln beskriver hur utvecklare kan lagra, hämta och fråga datum i Azure Cosmos-databasen med hello .NET SDK.

## <a name="storing-datetimes"></a>Lagra datum och tid
Som standard hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) Serialiserar DateTime-värden som [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strängar. De flesta program kan använda hello standard strängrepresentation för datum/tid för hello följande orsaker:

* Strängar kan jämföras och hello relativa sorteringen av hello DateTime-värden bevaras när de är transformerad toostrings. 
* Den här metoden kräver inte någon anpassad kod eller ett attribut för JSON-konvertering.
* hello datum som lagras i JSON är mänsklig läsbar.
* Den här metoden kan dra nytta av Azure Cosmos DB index för snabb frågeprestanda.

Till exempel hello följande fragment lagrar en `Order` objektegenskaper som innehåller två DateTime - `ShipDate` och `OrderDate` som ett dokument med hjälp av hello .NET SDK:

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

Det här dokumentet lagras i Azure Cosmos DB på följande sätt:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

Du kan också lagra datum och tid som Unix tidsstämplar som är som ett tal som representerar hello antal förflutna sekunder sedan den 1 januari 1970. Azure Cosmos DB interna tidsstämpel (`_ts`) egenskapen följer den här metoden. Du kan använda hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) klassen tooserialize datum och tid som tal. 

## <a name="indexing-datetimes-for-range-queries"></a>Indexering datum och tid för intervallet frågor
Intervallet frågor är vanliga med DateTime-värden. Om du behöver toofind alla order som skapats sedan igår eller söka efter alla order som levererats i hello senaste fem minuterna, måste till exempel tooperform intervallet frågor. tooexecute dessa frågor effektivt, måste du konfigurera din samling för intervallet indexering i strängar.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

Du kan lära dig mer om hur tooconfigure indexering principer på [Azure Cosmos DB indexering principer](indexing-policies.md).

## <a name="querying-datetimes-in-linq"></a>Frågar datum och tid i LINQ
hello .NET DocumentDB SDK har automatiskt stöd för hämtning av data som lagras i Azure Cosmos DB via LINQ. Hello visar följande utdrag exempelvis en LINQ-fråga som filter order som levererades i hello senaste tre dagarna.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated toohello following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

Du kan lära dig mer om Azure Cosmos DB SQL-frågan språk och hello LINQ-providern på [frågar Cosmos DB](documentdb-sql-query.md).

I den här artikeln vi har tittat på hur toostore, index- och frågar efter datum och tid i Azure Cosmos-databasen.

## <a name="next-steps"></a>Nästa steg
* Hämta och köra hello [kodexempel på GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* Lär dig mer om [DocumentDB API-fråga](documentdb-sql-query.md)
* Lär dig mer om [Azure Cosmos DB indexering principer](indexing-policies.md)
