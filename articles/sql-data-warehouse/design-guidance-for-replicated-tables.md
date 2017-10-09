---
title: "aaaDesign vägledning för replikerade tabeller - Azure SQL Data Warehouse | Microsoft Docs"
description: "Rekommendationer för hur man designar replikerade tabeller i Azure SQL Data Warehouse-schemat."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/14/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 5d405b8c404c65177b387ba959126839c1cf8799
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a><span data-ttu-id="82d93-103">Utforma riktlinjer för att använda replikerade tabeller i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="82d93-103">Design guidance for using replicated tables in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="82d93-104">Den här artikeln ger rekommendationer för att utforma replikerade tabeller i SQL Data Warehouse-schemat.</span><span class="sxs-lookup"><span data-stu-id="82d93-104">This article gives recommendations for designing replicated tables in your SQL Data Warehouse schema.</span></span> <span data-ttu-id="82d93-105">Använd dessa rekommendationer tooimprove frågeprestanda genom att minska komplexiteten för data movement och fråga.</span><span class="sxs-lookup"><span data-stu-id="82d93-105">Use these recommendations tooimprove query performance by reducing data movement and query complexity.</span></span>

> [!NOTE]
> <span data-ttu-id="82d93-106">hello är replikerad tabell för närvarande i förhandsversion.</span><span class="sxs-lookup"><span data-stu-id="82d93-106">hello replicated table feature is currently in public preview.</span></span> <span data-ttu-id="82d93-107">Vissa beteenden är ämne toochange.</span><span class="sxs-lookup"><span data-stu-id="82d93-107">Some behaviors are subject toochange.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="82d93-108">Krav</span><span class="sxs-lookup"><span data-stu-id="82d93-108">Prerequisites</span></span>
<span data-ttu-id="82d93-109">Den här artikeln förutsätter att du är bekant med datadistribution och begrepp för flytt av data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="82d93-109">This article assumes you are familiar with data distribution and data movement concepts in SQL Data Warehouse.</span></span>  <span data-ttu-id="82d93-110">Mer information finns i [Distributed data](sql-data-warehouse-distributed-data.md).</span><span class="sxs-lookup"><span data-stu-id="82d93-110">For more information, see [Distributed data](sql-data-warehouse-distributed-data.md).</span></span> 

<span data-ttu-id="82d93-111">Som en del av tabelldesign, Förstå så mycket som möjligt om dina data och hur data hello efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="82d93-111">As part of table design, understand as much as possible about your data and how hello data is queried.</span></span>  <span data-ttu-id="82d93-112">Till exempel tänka på följande:</span><span class="sxs-lookup"><span data-stu-id="82d93-112">For example, consider these questions:</span></span>

- <span data-ttu-id="82d93-113">Hur stor är hello tabellen?</span><span class="sxs-lookup"><span data-stu-id="82d93-113">How large is hello table?</span></span>   
- <span data-ttu-id="82d93-114">Hur ofta uppdateras hello tabellen?</span><span class="sxs-lookup"><span data-stu-id="82d93-114">How often is hello table refreshed?</span></span>   
- <span data-ttu-id="82d93-115">Måste jag fakta-och dimensionstabeller i ett informationslager?</span><span class="sxs-lookup"><span data-stu-id="82d93-115">Do I have fact and dimension tables in a data warehouse?</span></span>   

## <a name="what-is-a-replicated-table"></a><span data-ttu-id="82d93-116">Vad är en replikerad tabell?</span><span class="sxs-lookup"><span data-stu-id="82d93-116">What is a replicated table?</span></span>
<span data-ttu-id="82d93-117">En replikerad tabell har en fullständig kopia av hello-tabell som är tillgängliga på varje Compute-nod.</span><span class="sxs-lookup"><span data-stu-id="82d93-117">A replicated table has a full copy of hello table accessible on each Compute node.</span></span> <span data-ttu-id="82d93-118">Replikera en tabell tar bort hello måste tootransfer data mellan datornoderna innan ett join- eller aggregering.</span><span class="sxs-lookup"><span data-stu-id="82d93-118">Replicating a table removes hello need tootransfer data among Compute nodes before a join or aggregation.</span></span> <span data-ttu-id="82d93-119">Eftersom hello tabellen har flera kopior, replikerade tabeller som fungerar bäst när hello tabellstorlek är mindre än 2 GB komprimerat.</span><span class="sxs-lookup"><span data-stu-id="82d93-119">Since hello table has multiple copies, replicated tables work best when hello table size is less than 2 GB compressed.</span></span>

<span data-ttu-id="82d93-120">hello visar följande diagram en replikerad tabell som är tillgängligt på varje Compute-nod.</span><span class="sxs-lookup"><span data-stu-id="82d93-120">hello following diagram shows a replicated table that is accessible on each Compute node.</span></span> <span data-ttu-id="82d93-121">Hello replikerad tabell är fullständigt kopierade tooa distributionsdatabasen på varje beräkningsnod i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="82d93-121">In SQL Data Warehouse, hello replicated table is fully copied tooa distribution database on each Compute node.</span></span> 

<span data-ttu-id="82d93-122">![Replikerade tabellen](media/guidance-for-using-replicated-tables/replicated-table.png "replikerade tabell")</span><span class="sxs-lookup"><span data-stu-id="82d93-122">![Replicated table](media/guidance-for-using-replicated-tables/replicated-table.png "Replicated table")</span></span>  

<span data-ttu-id="82d93-123">Replikerade tabeller fungerar bra för små dimensionstabeller i ett stjärnschema.</span><span class="sxs-lookup"><span data-stu-id="82d93-123">Replicated tables work well for small dimension tables in a star schema.</span></span> <span data-ttu-id="82d93-124">Dimensionstabeller är vanligtvis av storlek som gör det möjligt toostore och underhålla flera kopior.</span><span class="sxs-lookup"><span data-stu-id="82d93-124">Dimension tables are usually of a size that makes it feasible toostore and maintain multiple copies.</span></span> <span data-ttu-id="82d93-125">Dimensioner lagra beskrivande data som ändras långsamt, till exempel kundens namn och adress och produktinformation.</span><span class="sxs-lookup"><span data-stu-id="82d93-125">Dimensions store descriptive data that changes slowly, such as customer name and address, and product details.</span></span> <span data-ttu-id="82d93-126">hello långsamt ändra uppbyggnad hello data leder toofewer ombyggnad av hello replikerad tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-126">hello slowly changing nature of hello data leads toofewer rebuilds of hello replicated table.</span></span> 

<span data-ttu-id="82d93-127">Överväg att använda en replikerad tabell när:</span><span class="sxs-lookup"><span data-stu-id="82d93-127">Consider using a replicated table when:</span></span>

- <span data-ttu-id="82d93-128">hello tabellstorleken på disken är mindre än 2 GB, oavsett hello antalet rader.</span><span class="sxs-lookup"><span data-stu-id="82d93-128">hello table size on disk is less than 2 GB, regardless of hello number of rows.</span></span> <span data-ttu-id="82d93-129">toofind hello storleken på en tabell kan du använda hello [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) kommando: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span><span class="sxs-lookup"><span data-stu-id="82d93-129">toofind hello size of a table, you can use hello [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) command: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span></span> 
- <span data-ttu-id="82d93-130">hello tabellen används i kopplingar som annars skulle kräva dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="82d93-130">hello table is used in joins that would otherwise require data movement.</span></span> <span data-ttu-id="82d93-131">Exempelvis kräver en koppling för hash-distribuerade tabeller dataflyttning när hello anslutande kolumner inte är hello samma kolumn för distribution.</span><span class="sxs-lookup"><span data-stu-id="82d93-131">For example, a join on hash-distributed tables requires data movement when hello joining columns are not hello same distribution column.</span></span> <span data-ttu-id="82d93-132">Om någon av hello distribuerade hash-tabeller är liten du en replikerad tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-132">If one of hello hash-distributed tables is small, consider a replicated table.</span></span> <span data-ttu-id="82d93-133">En koppling för en tabell som resursallokering kräver dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="82d93-133">A join on a round-robin table requires data movement.</span></span> <span data-ttu-id="82d93-134">Vi rekommenderar att du använder replikerade tabeller i stället för resursallokering tabeller i de flesta fall.</span><span class="sxs-lookup"><span data-stu-id="82d93-134">We recommend using replicated tables instead of round-robin tables in most cases.</span></span> 


<span data-ttu-id="82d93-135">Överväg att konvertera en befintlig distribuerad tabell tooa replikeras när:</span><span class="sxs-lookup"><span data-stu-id="82d93-135">Consider converting an existing distributed table tooa replicated table when:</span></span>

- <span data-ttu-id="82d93-136">Frågan planerar Använd dataflyttsåtgärderna som sänder hello data tooall hello Compute-noder.</span><span class="sxs-lookup"><span data-stu-id="82d93-136">Query plans use data movement operations that broadcast hello data tooall hello Compute nodes.</span></span> <span data-ttu-id="82d93-137">Hej BroadcastMoveOperation dyr och långsammare prestanda för frågor.</span><span class="sxs-lookup"><span data-stu-id="82d93-137">hello BroadcastMoveOperation is expensive and slows query performance.</span></span> <span data-ttu-id="82d93-138">tooview dataflyttsåtgärderna i frågeplaner, Använd [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="82d93-138">tooview data movement operations in query plans, use [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span></span>
 
<span data-ttu-id="82d93-139">Replikerade tabeller kan inte ge hello bästa möjliga prestanda när:</span><span class="sxs-lookup"><span data-stu-id="82d93-139">Replicated tables may not yield hello best query performance when:</span></span>

- <span data-ttu-id="82d93-140">hello-tabellen har ofta infoga, uppdatera och ta bort.</span><span class="sxs-lookup"><span data-stu-id="82d93-140">hello table has frequent insert, update, and delete operations.</span></span> <span data-ttu-id="82d93-141">Dessa data manipulation language (DML) åtgärder kräver en återskapning av hello replikerad tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-141">These data manipulation language (DML) operations require a rebuild of hello replicated table.</span></span> <span data-ttu-id="82d93-142">Återskapa kan ofta ge lägre prestanda.</span><span class="sxs-lookup"><span data-stu-id="82d93-142">Rebuilding frequently can cause slower performance.</span></span>
- <span data-ttu-id="82d93-143">hello datalagret skalas ofta.</span><span class="sxs-lookup"><span data-stu-id="82d93-143">hello data warehouse is scaled frequently.</span></span> <span data-ttu-id="82d93-144">Skala ett informationslager ändrar hello antal Compute-noder som har en återskapning.</span><span class="sxs-lookup"><span data-stu-id="82d93-144">Scaling a data warehouse changes hello number of Compute nodes, which incurs a rebuild.</span></span>
- <span data-ttu-id="82d93-145">hello-tabellen har ett stort antal kolumner, men dataåtgärder normalt kommer åt litet antal kolumner.</span><span class="sxs-lookup"><span data-stu-id="82d93-145">hello table has a large number of columns, but data operations typically access only a small number of columns.</span></span> <span data-ttu-id="82d93-146">I det här scenariot, istället för att replikera hello hela tabellen, det kan vara effektivare toohash distribuera hello tabellen och skapa ett index på hello ofta kolumner.</span><span class="sxs-lookup"><span data-stu-id="82d93-146">In this scenario, instead of replicating hello entire table, it might be more effective toohash distribute hello table, and then create an index on hello frequently accessed columns.</span></span> <span data-ttu-id="82d93-147">När en fråga kräver dataflyttning SQL Data Warehouse endast flyttar data i hello begärda kolumner.</span><span class="sxs-lookup"><span data-stu-id="82d93-147">When a query requires data movement, SQL Data Warehouse only moves data in hello requested columns.</span></span> 



## <a name="use-replicated-tables-with-simple-query-predicates"></a><span data-ttu-id="82d93-148">Använda replikerade tabeller med enkel fråga predikat</span><span class="sxs-lookup"><span data-stu-id="82d93-148">Use replicated tables with simple query predicates</span></span>
<span data-ttu-id="82d93-149">Innan du väljer toodistribute eller replikera en tabell tänka hello typer av frågor som du planerar toorun mot hello tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-149">Before you choose toodistribute or replicate a table, think about hello types of queries you plan toorun against hello table.</span></span> <span data-ttu-id="82d93-150">När det är möjligt</span><span class="sxs-lookup"><span data-stu-id="82d93-150">Whenever possible,</span></span>

- <span data-ttu-id="82d93-151">Använd replikerade tabeller för frågor med enkel fråga predikat, till exempel likhet eller olikhet.</span><span class="sxs-lookup"><span data-stu-id="82d93-151">Use replicated tables for queries with simple query predicates, such as equality or inequality.</span></span>
- <span data-ttu-id="82d93-152">Använd distribuerade tabeller för frågor med komplicerade frågan predikat, till exempel vill eller inte gillar.</span><span class="sxs-lookup"><span data-stu-id="82d93-152">Use distributed tables for queries with complex query predicates, such as LIKE or NOT LIKE.</span></span>

<span data-ttu-id="82d93-153">Processorintensiva frågor gör bäst ifrån sig när hello arbete distribueras över alla hello Compute-noder.</span><span class="sxs-lookup"><span data-stu-id="82d93-153">CPU-intensive queries perform best when hello work is distributed across all of hello Compute nodes.</span></span> <span data-ttu-id="82d93-154">Till exempel bättre frågor som körs beräkningar på varje rad i en tabell för distribuerade tabeller än replikerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="82d93-154">For example, queries that run computations on each row of a table perform better on distributed tables than replicated tables.</span></span> <span data-ttu-id="82d93-155">Eftersom en replikerad tabell lagras i sin helhet på varje beräkningsnod körs en processorintensiva fråga mot en replikerad tabell mot hello hela tabellen på varje beräkningsnod.</span><span class="sxs-lookup"><span data-stu-id="82d93-155">Since a replicated table is stored in full on each Compute node, a CPU-intensive query against a replicated table runs against hello entire table on every Compute node.</span></span> <span data-ttu-id="82d93-156">hello kan extra beräkning sakta frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="82d93-156">hello extra computation can slow query performance.</span></span>

<span data-ttu-id="82d93-157">Den här frågan har till exempel ett komplext predikat.</span><span class="sxs-lookup"><span data-stu-id="82d93-157">For example, this query has a complex predicate.</span></span>  <span data-ttu-id="82d93-158">Det körs snabbare när leverantören är en distribuerad tabell i stället för en replikerad tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-158">It runs faster when supplier is a distributed table instead of a replicated table.</span></span> <span data-ttu-id="82d93-159">Leverantören kan vara hash-distribuerade eller resursallokering distribueras i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="82d93-159">In this example, supplier can be hash-distributed or round-robin distributed.</span></span>

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-tooreplicated-tables"></a><span data-ttu-id="82d93-160">Konvertera befintliga resursallokering tabeller tooreplicated tabeller</span><span class="sxs-lookup"><span data-stu-id="82d93-160">Convert existing round-robin tables tooreplicated tables</span></span>
<span data-ttu-id="82d93-161">Om du redan har resursallokering tabeller, rekommenderar vi konvertera dem tooreplicated tabellerna om de uppfyller med villkor som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="82d93-161">If you already have round-robin tables, we recommend converting them tooreplicated tables if they meet with criteria outlined in this article.</span></span> <span data-ttu-id="82d93-162">Replikerade tabeller förbättra prestanda över resursallokering tabeller eftersom de undanröjer hello behovet av dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="82d93-162">Replicated tables improve performance over round-robin tables because they eliminate hello need for data movement.</span></span>  <span data-ttu-id="82d93-163">En tabell med resursallokering kräver alltid dataflyttning för kopplingar.</span><span class="sxs-lookup"><span data-stu-id="82d93-163">A round-robin table always requires data movement for joins.</span></span> 

<span data-ttu-id="82d93-164">Det här exemplet används [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory tabell tooa replikerad tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-164">This example uses [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory table tooa replicated table.</span></span> <span data-ttu-id="82d93-165">Det här exemplet fungerar oavsett om DimSalesTerritory är hash-distribuerade eller resursallokering.</span><span class="sxs-lookup"><span data-stu-id="82d93-165">This example works regardless of whether DimSalesTerritory is hash-distributed or round-robin.</span></span>

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] too[DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] too[DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a><span data-ttu-id="82d93-166">Exempel på frågan resultat för resursallokering jämfört med replikeras</span><span class="sxs-lookup"><span data-stu-id="82d93-166">Query performance example for round-robin versus replicated</span></span> 

<span data-ttu-id="82d93-167">En replikerad tabell kräver inte någon dataflyttning för kopplingar eftersom det redan finns på varje beräkningsnod hello hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="82d93-167">A replicated table does not require any data movement for joins because hello entire table is already present on each Compute node.</span></span> <span data-ttu-id="82d93-168">Om hello dimensionstabeller resursallokering distribueras, kopieras en koppling hälsningspaket dimensionstabellen i fullständig tooeach Compute-nod.</span><span class="sxs-lookup"><span data-stu-id="82d93-168">If hello dimension tables are round-robin distributed, a join copies hello dimension table in full tooeach Compute node.</span></span> <span data-ttu-id="82d93-169">toomove hello data hello frågeplan innehåller en åtgärd som kallas BroadcastMoveOperation.</span><span class="sxs-lookup"><span data-stu-id="82d93-169">toomove hello data, hello query plan contains an operation called BroadcastMoveOperation.</span></span> <span data-ttu-id="82d93-170">Den här typen av data movement åtgärden långsammare prestanda för frågor och elimineras med hjälp av replikerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="82d93-170">This type of data movement operation slows query performance and is eliminated by using replicated tables.</span></span> <span data-ttu-id="82d93-171">tooview för planen frågesteg använda hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) system katalogvyn.</span><span class="sxs-lookup"><span data-stu-id="82d93-171">tooview query plan steps, use hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) system catalog view.</span></span> 

<span data-ttu-id="82d93-172">Till exempel i följande fråga mot hello AdventureWorks schema hello ` FactInternetSales` tabellen är hash-distribueras.</span><span class="sxs-lookup"><span data-stu-id="82d93-172">For example, in following query against hello AdventureWorks schema, hello ` FactInternetSales` table is hash-distributed.</span></span> <span data-ttu-id="82d93-173">Hej `DimDate` och `DimSalesTerritory` tabeller är mindre dimensionstabeller.</span><span class="sxs-lookup"><span data-stu-id="82d93-173">hello `DimDate` and `DimSalesTerritory` tables are smaller dimension tables.</span></span> <span data-ttu-id="82d93-174">Den här frågan returnerar hello total försäljning i Nordamerika för räkenskapsår 2004:</span><span class="sxs-lookup"><span data-stu-id="82d93-174">This query returns hello total sales in North America for fiscal year 2004:</span></span>
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
<span data-ttu-id="82d93-175">Vi återskapas `DimDate` och `DimSalesTerritory` som resursallokering tabeller.</span><span class="sxs-lookup"><span data-stu-id="82d93-175">We re-created `DimDate` and `DimSalesTerritory` as round-robin tables.</span></span> <span data-ttu-id="82d93-176">Därför hello frågan visade hello följande frågeplan som har flera broadcast flytta operationer:</span><span class="sxs-lookup"><span data-stu-id="82d93-176">As a result, hello query showed hello following query plan, which has multiple broadcast move operations:</span></span> 
 
![Resursallokering frågeplan](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

<span data-ttu-id="82d93-178">Vi återskapas `DimDate` och `DimSalesTerritory` som replikerade tabeller och kördes hello frågan igen.</span><span class="sxs-lookup"><span data-stu-id="82d93-178">We re-created `DimDate` and `DimSalesTerritory` as replicated tables, and ran hello query again.</span></span> <span data-ttu-id="82d93-179">hello resulterande frågeplan är mycket kortare och inte har någon sänder ut flyttar.</span><span class="sxs-lookup"><span data-stu-id="82d93-179">hello resulting query plan is much shorter and does not have any broadcast moves.</span></span>

![Replikerade frågeplan](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a><span data-ttu-id="82d93-181">Prestandaöverväganden för att ändra replikerade tabeller</span><span class="sxs-lookup"><span data-stu-id="82d93-181">Performance considerations for modifying replicated tables</span></span>
<span data-ttu-id="82d93-182">SQL Data Warehouse implementerar en replikerad tabell genom att underhålla en huvudversionen av hello tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-182">SQL Data Warehouse implements a replicated table by maintaining a master version of hello table.</span></span> <span data-ttu-id="82d93-183">Den kopierar hello huvudversionen tooone distributionsdatabasen på varje beräkningsnod.</span><span class="sxs-lookup"><span data-stu-id="82d93-183">It copies hello master version tooone distribution database on each Compute node.</span></span> <span data-ttu-id="82d93-184">När det finns en ändring, uppdaterar SQL Data Warehouse först hello huvudtabellen.</span><span class="sxs-lookup"><span data-stu-id="82d93-184">When there is a change, SQL Data Warehouse first updates hello master table.</span></span> <span data-ttu-id="82d93-185">Sedan kräver den att hello tabeller på varje Compute-nod.</span><span class="sxs-lookup"><span data-stu-id="82d93-185">Then it requires a rebuild of hello tables on each Compute node.</span></span> <span data-ttu-id="82d93-186">En återskapning av en replikerad tabell innehåller kopiera hello tabell tooeach Compute-nod och sedan återskapa hello index.</span><span class="sxs-lookup"><span data-stu-id="82d93-186">A rebuild of a replicated table includes copying hello table tooeach Compute node and then rebuilding hello indexes.</span></span>

<span data-ttu-id="82d93-187">Ombyggnad krävs efter:</span><span class="sxs-lookup"><span data-stu-id="82d93-187">Rebuilds are required after:</span></span>
- <span data-ttu-id="82d93-188">Data läses in eller ändras</span><span class="sxs-lookup"><span data-stu-id="82d93-188">Data is loaded or modified</span></span>
- <span data-ttu-id="82d93-189">hello data warehouse är skalade tooa olika DWU-inställningar</span><span class="sxs-lookup"><span data-stu-id="82d93-189">hello data warehouse is scaled tooa different DWU setting</span></span>
- <span data-ttu-id="82d93-190">Tabelldefinitionen uppdateras</span><span class="sxs-lookup"><span data-stu-id="82d93-190">Table definition is updated</span></span>

<span data-ttu-id="82d93-191">Ombyggnad krävs inte efter:</span><span class="sxs-lookup"><span data-stu-id="82d93-191">Rebuilds are not required after:</span></span>
- <span data-ttu-id="82d93-192">Åtgärden pausa</span><span class="sxs-lookup"><span data-stu-id="82d93-192">Pause operation</span></span>
- <span data-ttu-id="82d93-193">Åtgärden återuppta</span><span class="sxs-lookup"><span data-stu-id="82d93-193">Resume operation</span></span>

<span data-ttu-id="82d93-194">hello återskapa sker inte omedelbart när data har ändrats.</span><span class="sxs-lookup"><span data-stu-id="82d93-194">hello rebuild does not happen immediately after data is modified.</span></span> <span data-ttu-id="82d93-195">I stället hello återskapa utlöses hello första gången en fråga markerar i hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="82d93-195">Instead, hello rebuild is triggered hello first time a query selects from hello table.</span></span>  <span data-ttu-id="82d93-196">Inom hello första select-instruktion från hello tabell finns steg toorebuild hello replikerad tabell.</span><span class="sxs-lookup"><span data-stu-id="82d93-196">Within hello initial select statement from hello table are steps toorebuild hello replicated table.</span></span>  <span data-ttu-id="82d93-197">Eftersom hello återskapa görs i hello-fråga, kan hello påverkan toohello inledande väljer instruktionen ha betydande beroende på hello storleken på hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="82d93-197">Because hello rebuild is done within hello query, hello impact toohello initial select statement could be significant depending on hello size of hello table.</span></span>  <span data-ttu-id="82d93-198">Om flera replikerade tabeller ingår som behöver en återskapning, återskapas varje kopia följd som steg i hello-instruktion.</span><span class="sxs-lookup"><span data-stu-id="82d93-198">If multiple replicated tables are involved that need a rebuild, each copy is rebuilt serially as steps within hello statement.</span></span>  <span data-ttu-id="82d93-199">toomaintain datakonsekvens under hello återskapa hello replikerad tabell ett exklusivt lås hämtas för hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="82d93-199">toomaintain data consistency during hello rebuild of hello replicated table an exclusive lock is taken on hello table.</span></span>  <span data-ttu-id="82d93-200">hello lås förhindrar alla åtkomst toohello tabellen för hello återskapa hello varaktighet.</span><span class="sxs-lookup"><span data-stu-id="82d93-200">hello lock prevents all access toohello table for hello duration of hello rebuild.</span></span> 

### <a name="use-indexes-conservatively"></a><span data-ttu-id="82d93-201">Använda index hänsyn</span><span class="sxs-lookup"><span data-stu-id="82d93-201">Use indexes conservatively</span></span>
<span data-ttu-id="82d93-202">Standard indexering praxis gäller tooreplicated tabeller.</span><span class="sxs-lookup"><span data-stu-id="82d93-202">Standard indexing practices apply tooreplicated tables.</span></span> <span data-ttu-id="82d93-203">SQL Data Warehouse återskapar varje replikerad Tabellindex som en del av hello återskapa.</span><span class="sxs-lookup"><span data-stu-id="82d93-203">SQL Data Warehouse rebuilds each replicated table index as part of hello rebuild.</span></span> <span data-ttu-id="82d93-204">Använd endast index om hello prestandafördelar uppväger hello kostnaden för att bygga om hello index.</span><span class="sxs-lookup"><span data-stu-id="82d93-204">Only use indexes when hello performance gain outweighs hello cost of rebuilding hello indexes.</span></span>  
 
### <a name="batch-data-loads"></a><span data-ttu-id="82d93-205">Batch databelastningar</span><span class="sxs-lookup"><span data-stu-id="82d93-205">Batch data loads</span></span>
<span data-ttu-id="82d93-206">När du läser in data i replikerade tabeller, kan du försöka toominimize bygger genom batchbearbetning belastningar tillsammans.</span><span class="sxs-lookup"><span data-stu-id="82d93-206">When loading data into replicated tables, try toominimize rebuilds by batching loads together.</span></span> <span data-ttu-id="82d93-207">Utför alla hello batchar belastningar innan du kör select-satser.</span><span class="sxs-lookup"><span data-stu-id="82d93-207">Perform all hello batched loads before running select statements.</span></span>

<span data-ttu-id="82d93-208">Det här mönstret belastningen läser in data från fyra källor och anropar fyra bygger.</span><span class="sxs-lookup"><span data-stu-id="82d93-208">For example, this load pattern loads data from four sources and invokes four rebuilds.</span></span> 

- <span data-ttu-id="82d93-209">Läsa in från datakällan 1.</span><span class="sxs-lookup"><span data-stu-id="82d93-209">Load from source 1.</span></span>
- <span data-ttu-id="82d93-210">SELECT-instruktion utlösare återskapa 1.</span><span class="sxs-lookup"><span data-stu-id="82d93-210">Select statement triggers rebuild 1.</span></span>
- <span data-ttu-id="82d93-211">Läsa in från källan 2.</span><span class="sxs-lookup"><span data-stu-id="82d93-211">Load from source 2.</span></span>
- <span data-ttu-id="82d93-212">SELECT-instruktion utlösare återskapa 2.</span><span class="sxs-lookup"><span data-stu-id="82d93-212">Select statement triggers rebuild 2.</span></span>
- <span data-ttu-id="82d93-213">Läsa in från källan 3.</span><span class="sxs-lookup"><span data-stu-id="82d93-213">Load from source 3.</span></span>
- <span data-ttu-id="82d93-214">SELECT-instruktion utlösare återskapa 3.</span><span class="sxs-lookup"><span data-stu-id="82d93-214">Select statement triggers rebuild 3.</span></span>
- <span data-ttu-id="82d93-215">Läsa in från datakällan 4.</span><span class="sxs-lookup"><span data-stu-id="82d93-215">Load from source 4.</span></span>
- <span data-ttu-id="82d93-216">SELECT-instruktion utlösare återskapa 4.</span><span class="sxs-lookup"><span data-stu-id="82d93-216">Select statement triggers rebuild 4.</span></span>

<span data-ttu-id="82d93-217">Det här mönstret belastningen läser in data från fyra källor men endast anropar en återskapning.</span><span class="sxs-lookup"><span data-stu-id="82d93-217">For example, this load pattern loads data from four sources, but only invokes one rebuild.</span></span>

- <span data-ttu-id="82d93-218">Läsa in från datakällan 1.</span><span class="sxs-lookup"><span data-stu-id="82d93-218">Load from source 1.</span></span>
- <span data-ttu-id="82d93-219">Läsa in från källan 2.</span><span class="sxs-lookup"><span data-stu-id="82d93-219">Load from source 2.</span></span>
- <span data-ttu-id="82d93-220">Läsa in från källan 3.</span><span class="sxs-lookup"><span data-stu-id="82d93-220">Load from source 3.</span></span>
- <span data-ttu-id="82d93-221">Läsa in från datakällan 4.</span><span class="sxs-lookup"><span data-stu-id="82d93-221">Load from source 4.</span></span>
- <span data-ttu-id="82d93-222">SELECT-instruktion utlösare återskapa.</span><span class="sxs-lookup"><span data-stu-id="82d93-222">Select statement triggers rebuild.</span></span>


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a><span data-ttu-id="82d93-223">Återskapa en replikerad tabell efter en batch-inläsning</span><span class="sxs-lookup"><span data-stu-id="82d93-223">Rebuild a replicated table after a batch load</span></span>
<span data-ttu-id="82d93-224">tooensure konsekvent körningstider, rekommenderar vi att tvinga en uppdatering av hello replikerade tabeller efter en batch inläsningen.</span><span class="sxs-lookup"><span data-stu-id="82d93-224">tooensure consistent query execution times, we recommend forcing a refresh of hello replicated tables after a batch load.</span></span> <span data-ttu-id="82d93-225">Annars måste hello första frågan vänta hello tabeller toorefresh som innehåller återskapa hello index.</span><span class="sxs-lookup"><span data-stu-id="82d93-225">Otherwise, hello first query must wait for hello tables toorefresh, which includes rebuilding hello indexes.</span></span> <span data-ttu-id="82d93-226">Beroende på hello antalet och storleken på replikerade tabeller påverkas, vara hello prestandapåverkan betydande.</span><span class="sxs-lookup"><span data-stu-id="82d93-226">Depending on hello size and number of replicated tables affected, hello performance impact can be significant.</span></span>  

<span data-ttu-id="82d93-227">Den här frågan använder hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replikerade tabeller som har ändrats, men inte återskapas.</span><span class="sxs-lookup"><span data-stu-id="82d93-227">This query uses hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replicated tables that have been modified, but not rebuilt.</span></span>

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
<span data-ttu-id="82d93-228">tooforce kör hello efter uttryck för varje tabell i hello föregående utdata för en återskapning.</span><span class="sxs-lookup"><span data-stu-id="82d93-228">tooforce a rebuild, run hello following statement on each table in hello preceding output.</span></span> 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a><span data-ttu-id="82d93-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82d93-229">Next steps</span></span> 
<span data-ttu-id="82d93-230">toocreate en replikerad tabell med någon av dessa uttryck:</span><span class="sxs-lookup"><span data-stu-id="82d93-230">toocreate a replicated table, use one of these statements:</span></span>

- [<span data-ttu-id="82d93-231">Skapa tabell (Azure SQL Data Warehouse)</span><span class="sxs-lookup"><span data-stu-id="82d93-231">CREATE TABLE (Azure SQL Data Warehouse)</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [<span data-ttu-id="82d93-232">Skapa TABLE AS SELECT (Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="82d93-232">CREATE TABLE AS SELECT (Azure SQL Data Warehouse</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

<span data-ttu-id="82d93-233">En översikt över distribuerade tabeller, se [distribuerade tabeller](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="82d93-233">For an overview of distributed tables, see [distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>



