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
# <a name="azure-cosmos-db-how-tooquery-table-data-by-using-hello-table-api-preview"></a><span data-ttu-id="9627a-104">Azure Cosmos DB: Hur tooquery tabelldata med hjälp av hello du tabellen API (förhandsgranskning)?</span><span class="sxs-lookup"><span data-stu-id="9627a-104">Azure Cosmos DB: How tooquery table data by using hello Table API (preview)?</span></span>

<span data-ttu-id="9627a-105">hello Azure Cosmos DB [tabell API](table-introduction.md) (förhandsversion) stöder OData och [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) frågor mot nyckel/värde-(tabell) data.</span><span class="sxs-lookup"><span data-stu-id="9627a-105">hello Azure Cosmos DB [Table API](table-introduction.md) (preview) supports OData and [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) queries against key/value (table) data.</span></span>  

<span data-ttu-id="9627a-106">Den här artikeln beskriver hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="9627a-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="9627a-107">Datafrågor med hello tabell-API</span><span class="sxs-lookup"><span data-stu-id="9627a-107">Querying data with hello Table API</span></span>

<span data-ttu-id="9627a-108">hello frågorna i den här artikeln använder hello följande exempel `People` tabell:</span><span class="sxs-lookup"><span data-stu-id="9627a-108">hello queries in this article use hello following sample `People` table:</span></span>

| <span data-ttu-id="9627a-109">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="9627a-109">PartitionKey</span></span> | <span data-ttu-id="9627a-110">RowKey</span><span class="sxs-lookup"><span data-stu-id="9627a-110">RowKey</span></span> | <span data-ttu-id="9627a-111">E-post</span><span class="sxs-lookup"><span data-stu-id="9627a-111">Email</span></span> | <span data-ttu-id="9627a-112">Telefonnummer</span><span class="sxs-lookup"><span data-stu-id="9627a-112">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9627a-113">Harp</span><span class="sxs-lookup"><span data-stu-id="9627a-113">Harp</span></span> | <span data-ttu-id="9627a-114">Viktor</span><span class="sxs-lookup"><span data-stu-id="9627a-114">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="9627a-115">425-555-0101</span><span class="sxs-lookup"><span data-stu-id="9627a-115">425-555-0101</span></span> |
| <span data-ttu-id="9627a-116">Smith</span><span class="sxs-lookup"><span data-stu-id="9627a-116">Smith</span></span> | <span data-ttu-id="9627a-117">Ben</span><span class="sxs-lookup"><span data-stu-id="9627a-117">Ben</span></span> | Ben@contoso.com| <span data-ttu-id="9627a-118">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="9627a-118">425-555-0102</span></span> |
| <span data-ttu-id="9627a-119">Smith</span><span class="sxs-lookup"><span data-stu-id="9627a-119">Smith</span></span> | <span data-ttu-id="9627a-120">Jeff</span><span class="sxs-lookup"><span data-stu-id="9627a-120">Jeff</span></span> | Jeff@contoso.com| <span data-ttu-id="9627a-121">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="9627a-121">425-555-0104</span></span> | 

<span data-ttu-id="9627a-122">Eftersom Azure Cosmos DB är kompatibel med hello Azure Table storage API: er, se [frågor till tabeller och enheter] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) mer information om hur tooquery med hjälp av hello Tabell-API.</span><span class="sxs-lookup"><span data-stu-id="9627a-122">Because Azure Cosmos DB is compatible with hello Azure Table storage APIs, see [Querying Tables and Entities] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) for details on how tooquery by using hello Table API.</span></span> 

<span data-ttu-id="9627a-123">Mer information om hello premium-funktioner som erbjuder Azure Cosmos DB finns [Azure Cosmos DB: tabellen API](table-introduction.md) och [utveckla med hello tabell API i .NET](tutorial-develop-table-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="9627a-123">For more information on hello premium capabilities that Azure Cosmos DB offers, see [Azure Cosmos DB: Table API](table-introduction.md) and [Develop with hello Table API in .NET](tutorial-develop-table-dotnet.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9627a-124">Krav</span><span class="sxs-lookup"><span data-stu-id="9627a-124">Prerequisites</span></span>

<span data-ttu-id="9627a-125">För dessa frågor toowork måste du har ett konto i Azure Cosmos DB och har entitetsdata i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="9627a-125">For these queries toowork, you must have an Azure Cosmos DB account and have entity data in hello container.</span></span> <span data-ttu-id="9627a-126">Har inte något av de?</span><span class="sxs-lookup"><span data-stu-id="9627a-126">Don't have any of those?</span></span> <span data-ttu-id="9627a-127">Fullständig hello [fem minuter långa quickstart](https://aka.ms/acdbtnetqs) eller hello [developer kursen](https://aka.ms/acdbtabletut) toocreate ett konto och fylla i databasen.</span><span class="sxs-lookup"><span data-stu-id="9627a-127">Complete hello [five-minute quickstart](https://aka.ms/acdbtnetqs) or hello [developer tutorial](https://aka.ms/acdbtabletut) toocreate an account and populate your database.</span></span>

## <a name="query-on-partitionkey-and-rowkey"></a><span data-ttu-id="9627a-128">Fråga om PartitionKey och RowKey</span><span class="sxs-lookup"><span data-stu-id="9627a-128">Query on PartitionKey and RowKey</span></span>
<span data-ttu-id="9627a-129">Eftersom hello PartitionKey och RowKey egenskaper utgör en entitets primärnyckel, kan du använda hello följa särskilda syntax tooidentify hello entitet:</span><span class="sxs-lookup"><span data-stu-id="9627a-129">Because hello PartitionKey and RowKey properties form an entity's primary key, you can use hello following special syntax tooidentify hello entity:</span></span> 

<span data-ttu-id="9627a-130">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="9627a-130">**Query**</span></span>

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
<span data-ttu-id="9627a-131">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="9627a-131">**Results**</span></span>

| <span data-ttu-id="9627a-132">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="9627a-132">PartitionKey</span></span> | <span data-ttu-id="9627a-133">RowKey</span><span class="sxs-lookup"><span data-stu-id="9627a-133">RowKey</span></span> | <span data-ttu-id="9627a-134">E-post</span><span class="sxs-lookup"><span data-stu-id="9627a-134">Email</span></span> | <span data-ttu-id="9627a-135">Telefonnummer</span><span class="sxs-lookup"><span data-stu-id="9627a-135">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9627a-136">Harp</span><span class="sxs-lookup"><span data-stu-id="9627a-136">Harp</span></span> | <span data-ttu-id="9627a-137">Viktor</span><span class="sxs-lookup"><span data-stu-id="9627a-137">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="9627a-138">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="9627a-138">425-555-0104</span></span> |

<span data-ttu-id="9627a-139">Du kan också ange dessa egenskaper som en del av hello `$filter` alternativ, som visas i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="9627a-139">Alternatively, you can specify these properties as part of hello `$filter` option, as shown in hello following section.</span></span> <span data-ttu-id="9627a-140">Observera att hello nyckelegenskapen namn och konstanta värden är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="9627a-140">Note that hello key property names and constant values are case-sensitive.</span></span> <span data-ttu-id="9627a-141">Både hello PartitionKey och RowKey egenskaper är av typen String.</span><span class="sxs-lookup"><span data-stu-id="9627a-141">Both hello PartitionKey and RowKey properties are of type String.</span></span> 

## <a name="query-by-using-an-odata-filter"></a><span data-ttu-id="9627a-142">Fråga med hjälp av en OData-filter</span><span class="sxs-lookup"><span data-stu-id="9627a-142">Query by using an OData filter</span></span>
<span data-ttu-id="9627a-143">Tänk reglerna när du konstruera en Filtersträng:</span><span class="sxs-lookup"><span data-stu-id="9627a-143">When you're constructing a filter string, keep these rules in mind:</span></span> 

* <span data-ttu-id="9627a-144">Använd hello logiska operatorer som definieras av hello OData-protokollspecifikationen toocompare ett värde för egenskapen tooa.</span><span class="sxs-lookup"><span data-stu-id="9627a-144">Use hello logical operators defined by hello OData Protocol Specification toocompare a property tooa value.</span></span> <span data-ttu-id="9627a-145">Observera att du inte kan jämföra ett egenskapen tooa dynamiskt värde.</span><span class="sxs-lookup"><span data-stu-id="9627a-145">Note that you can't compare a property tooa dynamic value.</span></span> <span data-ttu-id="9627a-146">En sida av hello uttrycket måste vara en konstant.</span><span class="sxs-lookup"><span data-stu-id="9627a-146">One side of hello expression must be a constant.</span></span> 
* <span data-ttu-id="9627a-147">hello egenskapsnamn, operatör och konstantvärde måste avgränsas med URL-kodade blanksteg.</span><span class="sxs-lookup"><span data-stu-id="9627a-147">hello property name, operator, and constant value must be separated by URL-encoded spaces.</span></span> <span data-ttu-id="9627a-148">Ett utrymme är URL-kodade som `%20`.</span><span class="sxs-lookup"><span data-stu-id="9627a-148">A space is URL-encoded as `%20`.</span></span> 
* <span data-ttu-id="9627a-149">Alla delar av hello filtersträngen är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="9627a-149">All parts of hello filter string are case-sensitive.</span></span> 
* <span data-ttu-id="9627a-150">hello konstant värde måste vara av hello samma datatyp som hello egenskap för hello tooreturn giltig filterresultaten.</span><span class="sxs-lookup"><span data-stu-id="9627a-150">hello constant value must be of hello same data type as hello property in order for hello filter tooreturn valid results.</span></span> <span data-ttu-id="9627a-151">Läs mer om stöds egenskapstyperna [hello förstå tabelltjänst-datamodellen](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span><span class="sxs-lookup"><span data-stu-id="9627a-151">For more information about supported property types, see [Understanding hello Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span></span> 

<span data-ttu-id="9627a-152">Här är en exempelfråga som visar hur toofilter av hello PartitionKey och e-egenskaper med hjälp av en OData `$filter`.</span><span class="sxs-lookup"><span data-stu-id="9627a-152">Here's an example query that shows how toofilter by hello PartitionKey and Email properties by using an OData `$filter`.</span></span>

<span data-ttu-id="9627a-153">**Fråga**</span><span class="sxs-lookup"><span data-stu-id="9627a-153">**Query**</span></span>

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

<span data-ttu-id="9627a-154">Mer information om hur tooconstruct filtrera uttryck för olika datatyper finns [frågor till tabeller och de entiteter](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span><span class="sxs-lookup"><span data-stu-id="9627a-154">For more information on how tooconstruct filter expressions for various data types, see [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span></span>

<span data-ttu-id="9627a-155">**Resultat**</span><span class="sxs-lookup"><span data-stu-id="9627a-155">**Results**</span></span>

| <span data-ttu-id="9627a-156">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="9627a-156">PartitionKey</span></span> | <span data-ttu-id="9627a-157">RowKey</span><span class="sxs-lookup"><span data-stu-id="9627a-157">RowKey</span></span> | <span data-ttu-id="9627a-158">E-post</span><span class="sxs-lookup"><span data-stu-id="9627a-158">Email</span></span> | <span data-ttu-id="9627a-159">Telefonnummer</span><span class="sxs-lookup"><span data-stu-id="9627a-159">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9627a-160">Ben</span><span class="sxs-lookup"><span data-stu-id="9627a-160">Ben</span></span> |<span data-ttu-id="9627a-161">Smith</span><span class="sxs-lookup"><span data-stu-id="9627a-161">Smith</span></span> | Ben@contoso.com| <span data-ttu-id="9627a-162">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="9627a-162">425-555-0102</span></span> |

## <a name="query-by-using-linq"></a><span data-ttu-id="9627a-163">Fråga med hjälp av LINQ</span><span class="sxs-lookup"><span data-stu-id="9627a-163">Query by using LINQ</span></span> 
<span data-ttu-id="9627a-164">Du kan också fråga med hjälp av LINQ som översätter toohello motsvarande OData-frågeuttryck.</span><span class="sxs-lookup"><span data-stu-id="9627a-164">You can also query by using LINQ, which translates toohello corresponding OData query expressions.</span></span> <span data-ttu-id="9627a-165">Här är ett exempel på hur hello toobuild frågor med hjälp av .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="9627a-165">Here's an example of how toobuild queries by using hello .NET SDK:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="9627a-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9627a-166">Next steps</span></span>

<span data-ttu-id="9627a-167">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="9627a-167">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9627a-168">Lärt dig hur tooquery med hjälp av hello tabell-API (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="9627a-168">Learned how tooquery by using hello Table API (preview)</span></span> 

<span data-ttu-id="9627a-169">Du kan nu fortsätta toohello nästa självstudiekurs toolearn hur toodistribute data globalt.</span><span class="sxs-lookup"><span data-stu-id="9627a-169">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9627a-170">Distribuera dina data globalt</span><span class="sxs-lookup"><span data-stu-id="9627a-170">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)
