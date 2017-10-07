---
title: aaaHow tooquery tabelldata i Azure Cosmos DB? | Microsoft Docs
description: "Läs tooquery tabelldata i Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: govindk
ms.openlocfilehash: 32526c3488c589c5be3a4a2f174aa769570f0c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-table-data-by-using-hello-table-api-preview"></a>Azure Cosmos DB: Hur tooquery tabelldata med hjälp av hello du tabellen API (förhandsgranskning)?

hello Azure Cosmos DB [tabell API](table-introduction.md) (förhandsversion) stöder OData och [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) frågor mot nyckel/värde-(tabell) data.  

Den här artikeln beskriver hello följande uppgifter: 

> [!div class="checklist"]
> * Datafrågor med hello tabell-API

hello frågorna i den här artikeln använder hello följande exempel `People` tabell:

| PartitionKey | RowKey | E-post | Telefonnummer |
| --- | --- | --- | --- |
| Harp | Viktor | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Eftersom Azure Cosmos DB är kompatibel med hello Azure Table storage API: er, se [frågor till tabeller och enheter] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) mer information om hur tooquery med hjälp av hello Tabell-API. 

Mer information om hello premium-funktioner som erbjuder Azure Cosmos DB finns [Azure Cosmos DB: tabellen API](table-introduction.md) och [utveckla med hello tabell API i .NET](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Krav

För dessa frågor toowork måste du har ett konto i Azure Cosmos DB och har entitetsdata i hello behållaren. Har inte något av de? Fullständig hello [fem minuter långa quickstart](https://aka.ms/acdbtnetqs) eller hello [developer kursen](https://aka.ms/acdbtabletut) toocreate ett konto och fylla i databasen.

## <a name="query-on-partitionkey-and-rowkey"></a>Fråga om PartitionKey och RowKey
Eftersom hello PartitionKey och RowKey egenskaper utgör en entitets primärnyckel, kan du använda hello följa särskilda syntax tooidentify hello entitet: 

**Fråga**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Resultat**

| PartitionKey | RowKey | E-post | Telefonnummer |
| --- | --- | --- | --- |
| Harp | Viktor | Walter@contoso.com| 425-555-0104 |

Du kan också ange dessa egenskaper som en del av hello `$filter` alternativ, som visas i följande avsnitt hello. Observera att hello nyckelegenskapen namn och konstanta värden är skiftlägeskänsliga. Både hello PartitionKey och RowKey egenskaper är av typen String. 

## <a name="query-by-using-an-odata-filter"></a>Fråga med hjälp av en OData-filter
Tänk reglerna när du konstruera en Filtersträng: 

* Använd hello logiska operatorer som definieras av hello OData-protokollspecifikationen toocompare ett värde för egenskapen tooa. Observera att du inte kan jämföra ett egenskapen tooa dynamiskt värde. En sida av hello uttrycket måste vara en konstant. 
* hello egenskapsnamn, operatör och konstantvärde måste avgränsas med URL-kodade blanksteg. Ett utrymme är URL-kodade som `%20`. 
* Alla delar av hello filtersträngen är skiftlägeskänsliga. 
* hello konstant värde måste vara av hello samma datatyp som hello egenskap för hello tooreturn giltig filterresultaten. Läs mer om stöds egenskapstyperna [hello förstå tabelltjänst-datamodellen](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Här är en exempelfråga som visar hur toofilter av hello PartitionKey och e-egenskaper med hjälp av en OData `$filter`.

**Fråga**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Mer information om hur tooconstruct filtrera uttryck för olika datatyper finns [frågor till tabeller och de entiteter](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Resultat**

| PartitionKey | RowKey | E-post | Telefonnummer |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>Fråga med hjälp av LINQ 
Du kan också fråga med hjälp av LINQ som översätter toohello motsvarande OData-frågeuttryck. Här är ett exempel på hur hello toobuild frågor med hjälp av .NET SDK:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Lärt dig hur tooquery med hjälp av hello tabell-API (förhandsgranskning) 

Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodistribute data globalt.

> [!div class="nextstepaction"]
> [Distribuera dina data globalt](tutorial-global-distribution-documentdb.md)
