---
title: "Guide för att använda PolyBase i SQL Data Warehouse | Microsoft Docs"
description: "Riktlinjer och rekommendationer för att använda PolyBase i SQL Data Warehouse-scenarier."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 6/5/2016
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: 6938b92d8e5b46d908dc5b2155bdfdc89bb1dc8c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a><span data-ttu-id="2064e-103">Guide för att använda PolyBase i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2064e-103">Guide for using PolyBase in SQL Data Warehouse</span></span>
<span data-ttu-id="2064e-104">Den här handboken innehåller praktisk information för att använda PolyBase i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2064e-104">This guide gives practical information for using PolyBase in SQL Data Warehouse.</span></span>

<span data-ttu-id="2064e-105">Om du vill komma igång finns det [Läs in data med PolyBase] [ Load data with PolyBase] kursen.</span><span class="sxs-lookup"><span data-stu-id="2064e-105">To get started, see the [Load data with PolyBase][Load data with PolyBase] tutorial.</span></span>

## <a name="rotating-storage-keys"></a><span data-ttu-id="2064e-106">Rotera lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="2064e-106">Rotating storage keys</span></span>
<span data-ttu-id="2064e-107">Då kommer du vill ändra åtkomstnyckeln för till blob-lagring av säkerhetsskäl.</span><span class="sxs-lookup"><span data-stu-id="2064e-107">From time to time you will want to change the access key to your blob storage for security reasons.</span></span>

<span data-ttu-id="2064e-108">Det mest elegant sättet att utföra den här uppgiften är att följa en process som kallas ”rotera nycklarna”.</span><span class="sxs-lookup"><span data-stu-id="2064e-108">The most elegant way to perform this task is to follow a process known as "rotating the keys".</span></span> <span data-ttu-id="2064e-109">Kanske såg du att du har två lagringsnycklar för blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2064e-109">You may have noticed that you have two storage keys for your blob storage account.</span></span> <span data-ttu-id="2064e-110">Detta är så att du kan överföra</span><span class="sxs-lookup"><span data-stu-id="2064e-110">This is so that you can transition</span></span>

<span data-ttu-id="2064e-111">Rotera dina nycklar för Azure storage-konto är tre enkla steg</span><span class="sxs-lookup"><span data-stu-id="2064e-111">Rotating your Azure storage account keys is a simple three step process</span></span>

1. <span data-ttu-id="2064e-112">Skapa andra omfång databasautentiseringsuppgiften baserat på den sekundära lagringsåtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="2064e-112">Create second database scoped credential based on the secondary storage access key</span></span>
2. <span data-ttu-id="2064e-113">Skapa extern datakälla baserat på den här nya autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="2064e-113">Create second external data source based off this new credential</span></span>
3. <span data-ttu-id="2064e-114">Ta bort och skapa den externa tabeller som pekar på den externa datakällan</span><span class="sxs-lookup"><span data-stu-id="2064e-114">Drop and create the external table(s) pointing to the new external data source</span></span>

<span data-ttu-id="2064e-115">När du har migrerat alla externa tabeller till den externa datakällan och sedan kan du utföra rensningen uppgifter:</span><span class="sxs-lookup"><span data-stu-id="2064e-115">When you have migrated all your external tables to the new external data source then you can perform the clean up tasks:</span></span>

1. <span data-ttu-id="2064e-116">Släpp första extern datakälla</span><span class="sxs-lookup"><span data-stu-id="2064e-116">Drop first external data source</span></span>
2. <span data-ttu-id="2064e-117">Släpp första databas-omfattande autentisering baserat på den primära lagringsåtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="2064e-117">Drop first database scoped credential based on the primary storage access key</span></span>
3. <span data-ttu-id="2064e-118">Logga in på Azure och återskapa den primära åtkomstnyckeln som är redo för nästa gång</span><span class="sxs-lookup"><span data-stu-id="2064e-118">Log into Azure and regenerate the primary access key ready for the next time</span></span>

## <a name="query-azure-blob-storage-data"></a><span data-ttu-id="2064e-119">Frågan Azure blob storage-data</span><span class="sxs-lookup"><span data-stu-id="2064e-119">Query Azure blob storage data</span></span>
<span data-ttu-id="2064e-120">Frågor mot externa tabeller Använd bara tabellnamnet som om det var en relationella tabell.</span><span class="sxs-lookup"><span data-stu-id="2064e-120">Queries against external tables simply use the table name as though it was a relational table.</span></span>

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> <span data-ttu-id="2064e-121">En fråga på en extern tabell kan misslyckas med fel *”frågan avbröts--avvisa den maximala tröskelvärdet nåddes vid läsning från en extern källa”*.</span><span class="sxs-lookup"><span data-stu-id="2064e-121">A query on an external table can fail with the error *"Query aborted-- the maximum reject threshold was reached while reading from an external source"*.</span></span> <span data-ttu-id="2064e-122">Detta anger att externa data innehåller *felaktig* poster.</span><span class="sxs-lookup"><span data-stu-id="2064e-122">This indicates that your external data contains *dirty* records.</span></span> <span data-ttu-id="2064e-123">”Felaktig' anses vara en post om de faktiska data typer/antalet kolumner inte matchar kolumndefinitioner för extern tabell eller om data inte överensstämmer med det angivna externa filformatet.</span><span class="sxs-lookup"><span data-stu-id="2064e-123">A data record is considered 'dirty' if the actual data types/number of columns do not match the column definitions of the external table or if the data doesn't conform to the specified external file format.</span></span> <span data-ttu-id="2064e-124">Kontrollera att din extern tabell och definitioner för externa filformat är korrekta och externa data överensstämmer med dessa definitioner för att åtgärda detta.</span><span class="sxs-lookup"><span data-stu-id="2064e-124">To fix this, ensure that your external table and external file format definitions are correct and your external data conforms to these definitions.</span></span> <span data-ttu-id="2064e-125">Om en delmängd av externa dataposter är inaktuella kan du avvisa posterna för dina frågor genom att använda avvisa i Skapa extern tabell DDL.</span><span class="sxs-lookup"><span data-stu-id="2064e-125">In case a subset of external data records are dirty, you can choose to reject these records for your queries by using the reject options in CREATE EXTERNAL TABLE DDL.</span></span>
> 
> 

## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="2064e-126">Läsa in data från Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="2064e-126">Load data from Azure blob storage</span></span>
<span data-ttu-id="2064e-127">Det här exemplet läser data från Azure blob storage till SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="2064e-127">This example loads data from Azure blob storage to SQL Data Warehouse database.</span></span>

<span data-ttu-id="2064e-128">Lagra data direkt tar bort dataöverföringstid för frågor.</span><span class="sxs-lookup"><span data-stu-id="2064e-128">Storing data directly removes the data transfer time for queries.</span></span> <span data-ttu-id="2064e-129">Lagra data med ett columnstore-index förbättrar prestanda för analys av upp till 10 x.</span><span class="sxs-lookup"><span data-stu-id="2064e-129">Storing data with a columnstore index improves query performance for analysis queries by up to 10x.</span></span>

<span data-ttu-id="2064e-130">Det här exemplet använder CREATE TABLE AS SELECT-instruktion för att läsa in data.</span><span class="sxs-lookup"><span data-stu-id="2064e-130">This example uses the CREATE TABLE AS SELECT statement to load data.</span></span> <span data-ttu-id="2064e-131">Den nya tabellen ärver kolumnerna som namnges i frågan.</span><span class="sxs-lookup"><span data-stu-id="2064e-131">The new table inherits the columns named in the query.</span></span> <span data-ttu-id="2064e-132">Datatyperna för kolumnerna ärver från den externa tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="2064e-132">It inherits the data types of those columns from the external table definition.</span></span>

<span data-ttu-id="2064e-133">CREATE TABLE AS SELECT är en hög performant Transact-SQL-uttryck som läser in data i parallellt datornoderna för ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2064e-133">CREATE TABLE AS SELECT is a highly performant Transact-SQL statement that loads the data in parallel to all the compute nodes of your SQL Data Warehouse.</span></span>  <span data-ttu-id="2064e-134">Den ursprungligen utvecklades för massivt parallell bearbetning (MPP) motorn i Analytics Platform System och är nu i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2064e-134">It was originally developed for  the massively parallel processing (MPP) engine in Analytics Platform System and is now in SQL Data Warehouse.</span></span>

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

<span data-ttu-id="2064e-135">Se [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="2064e-135">See [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span></span>

## <a name="create-statistics-on-newly-loaded-data"></a><span data-ttu-id="2064e-136">Skapa statistik på nyinlästa data</span><span class="sxs-lookup"><span data-stu-id="2064e-136">Create Statistics on newly loaded data</span></span>
<span data-ttu-id="2064e-137">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="2064e-137">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="2064e-138">För att få bästa möjliga prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter den första inläsningen eller vid betydande dataändringar.</span><span class="sxs-lookup"><span data-stu-id="2064e-138">In order to get the best performance from your queries, it's important that statistics be created on all columns of all tables after the first load or any substantial changes occur in the data.</span></span>  <span data-ttu-id="2064e-139">En detaljerad förklaring av statistik finns i ämnet [Statistik][Statistics] i ämnesgruppen Utveckla.</span><span class="sxs-lookup"><span data-stu-id="2064e-139">For a detailed explanation of statistics, see the [Statistics][Statistics] topic in the Develop group of topics.</span></span>  <span data-ttu-id="2064e-140">Nedan visas ett enkelt exempel på hur du skapar statistik på tabellerna som lästs in i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="2064e-140">Below is a quick example of how to create statistics on the tabled loaded in this example.</span></span>

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a><span data-ttu-id="2064e-141">Exportera data till Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="2064e-141">Export data to Azure blob storage</span></span>
<span data-ttu-id="2064e-142">Det här avsnittet visar hur du exporterar data från SQL Data Warehouse till Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="2064e-142">This section shows how to export data from SQL Data Warehouse to Azure blob storage.</span></span> <span data-ttu-id="2064e-143">Det här exemplet använder skapa externa TABLE AS SELECT som är en hög performant Transact-SQL-instruktionen för att exportera data parallellt från compute-noder.</span><span class="sxs-lookup"><span data-stu-id="2064e-143">This example uses CREATE EXTERNAL TABLE AS SELECT which is a highly performant Transact-SQL statement to export the data in parallel from all the compute nodes.</span></span>

<span data-ttu-id="2064e-144">I följande exempel skapas en extern tabell Weblogs2014 med kolumndefinitionerna och data från dbo. Webbloggar tabell.</span><span class="sxs-lookup"><span data-stu-id="2064e-144">The following example creates an external table Weblogs2014 using column definitions and data from dbo.Weblogs table.</span></span> <span data-ttu-id="2064e-145">Den externa tabelldefinitionen lagras i SQL Data Warehouse och resultatet av SELECT-instruktionen exporteras till katalogen ”/ Arkivera/log2014 /” i blob-behållaren som anges av datakällan.</span><span class="sxs-lookup"><span data-stu-id="2064e-145">The external table definition is stored in SQL Data Warehouse and the results of the SELECT statement are exported to the "/archive/log2014/" directory under the blob container specified by the data source.</span></span> <span data-ttu-id="2064e-146">Data exporteras i formatet angiven text.</span><span class="sxs-lookup"><span data-stu-id="2064e-146">The data is exported in the specified text file format.</span></span>

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a><span data-ttu-id="2064e-147">Isolera läsa in användare</span><span class="sxs-lookup"><span data-stu-id="2064e-147">Isolate Loading Users</span></span>
<span data-ttu-id="2064e-148">Det är ofta behöver flera användare som kan läsa in data i en SQL DW.</span><span class="sxs-lookup"><span data-stu-id="2064e-148">There is often a need to have multiple users that can load data into a SQL DW.</span></span> <span data-ttu-id="2064e-149">Eftersom den [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] kräver behörighet på databasen, du kommer att få flera användare med åtkomst kontroll över alla scheman.</span><span class="sxs-lookup"><span data-stu-id="2064e-149">Because the [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] requires CONTROL permissions of the database, you will end up with multiple users with control access over all schemas.</span></span> <span data-ttu-id="2064e-150">Du kan använda instruktionen NEKA kontroll för att begränsa detta.</span><span class="sxs-lookup"><span data-stu-id="2064e-150">To limit this, you can use the DENY CONTROL statement.</span></span>

<span data-ttu-id="2064e-151">Exempel: Överväg att databasen scheman schema_A för Avd A och schema_B för Avd B låt databasen användare user_A och user_B vara användare för PolyBase läser in i Avd A och B, respektive.</span><span class="sxs-lookup"><span data-stu-id="2064e-151">Example: Consider database schemas schema_A for dept A, and schema_B for dept B Let database users user_A and user_B be users for PolyBase loading in dept A and B, respectively.</span></span> <span data-ttu-id="2064e-152">De båda ha beviljats behörighet för databasen.</span><span class="sxs-lookup"><span data-stu-id="2064e-152">They both have been granted CONTROL database permissions.</span></span>
<span data-ttu-id="2064e-153">Skapare av A och B nu schemalås ned sina scheman med NEKA:</span><span class="sxs-lookup"><span data-stu-id="2064e-153">The creators of schema A and B now lock down their schemas using DENY:</span></span>

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   
 <span data-ttu-id="2064e-154">Med den här, bör user_A och user_B nu utelåst från andra Avd schema.</span><span class="sxs-lookup"><span data-stu-id="2064e-154">With this, user_A and user_B should now be locked out from the other dept’s schema.</span></span>
 


## <a name="next-steps"></a><span data-ttu-id="2064e-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2064e-155">Next steps</span></span>
<span data-ttu-id="2064e-156">Mer information om hur du flyttar data till SQL Data Warehouse finns i [översikt över datamigrering][data migration overview].</span><span class="sxs-lookup"><span data-stu-id="2064e-156">To learn more about moving data to SQL Data Warehouse, see the [data migration overview][data migration overview].</span></span>

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
