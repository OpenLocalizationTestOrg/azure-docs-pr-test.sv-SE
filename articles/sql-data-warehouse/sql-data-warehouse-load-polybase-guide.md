---
title: "aaaGuide för att använda PolyBase i SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: b05e4c5d528f2fe1c60d6855b5333065f0c908ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a><span data-ttu-id="45310-103">Guide för att använda PolyBase i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="45310-103">Guide for using PolyBase in SQL Data Warehouse</span></span>
<span data-ttu-id="45310-104">Den här handboken innehåller praktisk information för att använda PolyBase i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="45310-104">This guide gives practical information for using PolyBase in SQL Data Warehouse.</span></span>

<span data-ttu-id="45310-105">tooget igång, se hello [Läs in data med PolyBase] [ Load data with PolyBase] kursen.</span><span class="sxs-lookup"><span data-stu-id="45310-105">tooget started, see hello [Load data with PolyBase][Load data with PolyBase] tutorial.</span></span>

## <a name="rotating-storage-keys"></a><span data-ttu-id="45310-106">Rotera lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="45310-106">Rotating storage keys</span></span>
<span data-ttu-id="45310-107">Från tid tootime ska toochange hello access key tooyour blob-lagring av säkerhetsskäl.</span><span class="sxs-lookup"><span data-stu-id="45310-107">From time tootime you will want toochange hello access key tooyour blob storage for security reasons.</span></span>

<span data-ttu-id="45310-108">Hej mest elegant sätt tooperform uppgiften är toofollow en process som kallas ”rotera nycklar hello”.</span><span class="sxs-lookup"><span data-stu-id="45310-108">hello most elegant way tooperform this task is toofollow a process known as "rotating hello keys".</span></span> <span data-ttu-id="45310-109">Kanske såg du att du har två lagringsnycklar för blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="45310-109">You may have noticed that you have two storage keys for your blob storage account.</span></span> <span data-ttu-id="45310-110">Detta är så att du kan överföra</span><span class="sxs-lookup"><span data-stu-id="45310-110">This is so that you can transition</span></span>

<span data-ttu-id="45310-111">Rotera dina nycklar för Azure storage-konto är tre enkla steg</span><span class="sxs-lookup"><span data-stu-id="45310-111">Rotating your Azure storage account keys is a simple three step process</span></span>

1. <span data-ttu-id="45310-112">Skapa andra omfång databasautentiseringsuppgiften baserat på hello sekundära lagringsåtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="45310-112">Create second database scoped credential based on hello secondary storage access key</span></span>
2. <span data-ttu-id="45310-113">Skapa extern datakälla baserat på den här nya autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="45310-113">Create second external data source based off this new credential</span></span>
3. <span data-ttu-id="45310-114">Ta bort och skapa hello externa tabeller pekar toohello nya extern datakälla</span><span class="sxs-lookup"><span data-stu-id="45310-114">Drop and create hello external table(s) pointing toohello new external data source</span></span>

<span data-ttu-id="45310-115">När du har migrerat alla externa tabeller toohello nya externa datakällan och sedan kan du utföra hello Rensa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="45310-115">When you have migrated all your external tables toohello new external data source then you can perform hello clean up tasks:</span></span>

1. <span data-ttu-id="45310-116">Släpp första extern datakälla</span><span class="sxs-lookup"><span data-stu-id="45310-116">Drop first external data source</span></span>
2. <span data-ttu-id="45310-117">Släpp första databas-omfattande autentisering baserat på hello primära lagringsåtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="45310-117">Drop first database scoped credential based on hello primary storage access key</span></span>
3. <span data-ttu-id="45310-118">Logga in på Azure och återskapa hello primärnyckeln redo för hello nästa gång</span><span class="sxs-lookup"><span data-stu-id="45310-118">Log into Azure and regenerate hello primary access key ready for hello next time</span></span>

## <a name="query-azure-blob-storage-data"></a><span data-ttu-id="45310-119">Frågan Azure blob storage-data</span><span class="sxs-lookup"><span data-stu-id="45310-119">Query Azure blob storage data</span></span>
<span data-ttu-id="45310-120">Frågor mot externa tabeller Använd bara hello tabellnamn som om det var en relationella tabell.</span><span class="sxs-lookup"><span data-stu-id="45310-120">Queries against external tables simply use hello table name as though it was a relational table.</span></span>

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> <span data-ttu-id="45310-121">En fråga på en extern tabell kan misslyckas med hello fel *”frågan avbröts--hello avvisa den maximala tröskelvärdet nåddes vid läsning från en extern källa”*.</span><span class="sxs-lookup"><span data-stu-id="45310-121">A query on an external table can fail with hello error *"Query aborted-- hello maximum reject threshold was reached while reading from an external source"*.</span></span> <span data-ttu-id="45310-122">Detta anger att externa data innehåller *felaktig* poster.</span><span class="sxs-lookup"><span data-stu-id="45310-122">This indicates that your external data contains *dirty* records.</span></span> <span data-ttu-id="45310-123">”Felaktig' anses vara en post om hello faktiska data typer/antal kolumner inte matchar hello kolumndefinitioner för hello extern tabell eller om hello data stämmer inte överens toohello angivna externa filformatet.</span><span class="sxs-lookup"><span data-stu-id="45310-123">A data record is considered 'dirty' if hello actual data types/number of columns do not match hello column definitions of hello external table or if hello data doesn't conform toohello specified external file format.</span></span> <span data-ttu-id="45310-124">toofix, se till att en extern tabell och definitioner för externa filformat är korrekta och externa data överensstämmer toothese definitioner.</span><span class="sxs-lookup"><span data-stu-id="45310-124">toofix this, ensure that your external table and external file format definitions are correct and your external data conforms toothese definitions.</span></span> <span data-ttu-id="45310-125">Om en delmängd av externa dataposter är inaktuella kan du välja tooreject posterna för dina frågor med hello avvisa Skapa extern tabell DDL.</span><span class="sxs-lookup"><span data-stu-id="45310-125">In case a subset of external data records are dirty, you can choose tooreject these records for your queries by using hello reject options in CREATE EXTERNAL TABLE DDL.</span></span>
> 
> 

## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="45310-126">Läsa in data från Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="45310-126">Load data from Azure blob storage</span></span>
<span data-ttu-id="45310-127">Det här exemplet läser in data från Azure blob storage tooSQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="45310-127">This example loads data from Azure blob storage tooSQL Data Warehouse database.</span></span>

<span data-ttu-id="45310-128">Lagra data direkt tar bort hello dataöverföringstid för frågor.</span><span class="sxs-lookup"><span data-stu-id="45310-128">Storing data directly removes hello data transfer time for queries.</span></span> <span data-ttu-id="45310-129">Lagra data med ett columnstore-index förbättrar prestanda för analys av in too10x.</span><span class="sxs-lookup"><span data-stu-id="45310-129">Storing data with a columnstore index improves query performance for analysis queries by up too10x.</span></span>

<span data-ttu-id="45310-130">Det här exemplet använder hello CREATE TABLE AS SELECT-instruktionen tooload data.</span><span class="sxs-lookup"><span data-stu-id="45310-130">This example uses hello CREATE TABLE AS SELECT statement tooload data.</span></span> <span data-ttu-id="45310-131">hello ny tabell ärver hello kolumnerna som namnges i frågan hello.</span><span class="sxs-lookup"><span data-stu-id="45310-131">hello new table inherits hello columns named in hello query.</span></span> <span data-ttu-id="45310-132">Hello datatyperna för kolumnerna ärver från hello externa tabelldefinitionen.</span><span class="sxs-lookup"><span data-stu-id="45310-132">It inherits hello data types of those columns from hello external table definition.</span></span>

<span data-ttu-id="45310-133">CREATE TABLE AS SELECT är en hög performant Transact-SQL-uttryck som läser in hello data i parallella tooall hello compute-noder i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="45310-133">CREATE TABLE AS SELECT is a highly performant Transact-SQL statement that loads hello data in parallel tooall hello compute nodes of your SQL Data Warehouse.</span></span>  <span data-ttu-id="45310-134">Den ursprungligen utvecklades för hello massivt parallell bearbetning (MPP) motorn i Analytics Platform System och är nu i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="45310-134">It was originally developed for  hello massively parallel processing (MPP) engine in Analytics Platform System and is now in SQL Data Warehouse.</span></span>

```sql
-- Load data from Azure blob storage tooSQL Data Warehouse

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

<span data-ttu-id="45310-135">Se [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="45310-135">See [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span></span>

## <a name="create-statistics-on-newly-loaded-data"></a><span data-ttu-id="45310-136">Skapa statistik på nyinlästa data</span><span class="sxs-lookup"><span data-stu-id="45310-136">Create Statistics on newly loaded data</span></span>
<span data-ttu-id="45310-137">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="45310-137">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="45310-138">I ordning tooget hello bästa prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data.</span><span class="sxs-lookup"><span data-stu-id="45310-138">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="45310-139">En detaljerad förklaring av statistik finns hello [statistik] [ Statistics] -avsnittet i hello ämnesgruppen.</span><span class="sxs-lookup"><span data-stu-id="45310-139">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span>  <span data-ttu-id="45310-140">Nedan visas ett enkelt exempel på hur toocreate statistik hello fram läses in i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="45310-140">Below is a quick example of how toocreate statistics on hello tabled loaded in this example.</span></span>

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-tooazure-blob-storage"></a><span data-ttu-id="45310-141">Exportera data tooAzure blob-lagring</span><span class="sxs-lookup"><span data-stu-id="45310-141">Export data tooAzure blob storage</span></span>
<span data-ttu-id="45310-142">Det här avsnittet visar hur tooexport data från SQL Data Warehouse tooAzure blob storage.</span><span class="sxs-lookup"><span data-stu-id="45310-142">This section shows how tooexport data from SQL Data Warehouse tooAzure blob storage.</span></span> <span data-ttu-id="45310-143">Det här exemplet använder skapa externa TABLE AS SELECT som är en hög performant Transact-SQL-instruktionen tooexport hello data parallellt från alla hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="45310-143">This example uses CREATE EXTERNAL TABLE AS SELECT which is a highly performant Transact-SQL statement tooexport hello data in parallel from all hello compute nodes.</span></span>

<span data-ttu-id="45310-144">hello skapas följande exempel en extern tabell Weblogs2014 med kolumndefinitionerna och data från dbo. Webbloggar tabell.</span><span class="sxs-lookup"><span data-stu-id="45310-144">hello following example creates an external table Weblogs2014 using column definitions and data from dbo.Weblogs table.</span></span> <span data-ttu-id="45310-145">hello externa tabelldefinitionen lagras i SQL Data Warehouse och hello hello SELECT-instruktionen är exporterade toohello ”/ Arkivera/log2014 /” katalog under hello blob-behållare som angavs av hello datakällan.</span><span class="sxs-lookup"><span data-stu-id="45310-145">hello external table definition is stored in SQL Data Warehouse and hello results of hello SELECT statement are exported toohello "/archive/log2014/" directory under hello blob container specified by hello data source.</span></span> <span data-ttu-id="45310-146">hello data exporteras i hello angiven text-format.</span><span class="sxs-lookup"><span data-stu-id="45310-146">hello data is exported in hello specified text file format.</span></span>

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
## <a name="isolate-loading-users"></a><span data-ttu-id="45310-147">Isolera läsa in användare</span><span class="sxs-lookup"><span data-stu-id="45310-147">Isolate Loading Users</span></span>
<span data-ttu-id="45310-148">Det är ofta en toohave behovet av flera användare som kan läsa in data i en SQL DW.</span><span class="sxs-lookup"><span data-stu-id="45310-148">There is often a need toohave multiple users that can load data into a SQL DW.</span></span> <span data-ttu-id="45310-149">Eftersom hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] kräver behörighet av hello databasen du kommer att få flera användare med behörighet över alla scheman.</span><span class="sxs-lookup"><span data-stu-id="45310-149">Because hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] requires CONTROL permissions of hello database, you will end up with multiple users with control access over all schemas.</span></span> <span data-ttu-id="45310-150">toolimit, du kan använda hello NEKA kontroll.</span><span class="sxs-lookup"><span data-stu-id="45310-150">toolimit this, you can use hello DENY CONTROL statement.</span></span>

<span data-ttu-id="45310-151">Exempel: Överväg att databasen scheman schema_A för Avd A och schema_B för Avd B låt databasen användare user_A och user_B vara användare för PolyBase läser in i Avd A och B, respektive.</span><span class="sxs-lookup"><span data-stu-id="45310-151">Example: Consider database schemas schema_A for dept A, and schema_B for dept B Let database users user_A and user_B be users for PolyBase loading in dept A and B, respectively.</span></span> <span data-ttu-id="45310-152">De båda ha beviljats behörighet för databasen.</span><span class="sxs-lookup"><span data-stu-id="45310-152">They both have been granted CONTROL database permissions.</span></span>
<span data-ttu-id="45310-153">hello skapare av A och B nu låsa deras scheman med NEKA-schemat:</span><span class="sxs-lookup"><span data-stu-id="45310-153">hello creators of schema A and B now lock down their schemas using DENY:</span></span>

```sql
   DENY CONTROL ON SCHEMA :: schema_A toouser_B;
   DENY CONTROL ON SCHEMA :: schema_B toouser_A;
```   
 <span data-ttu-id="45310-154">Med den här, user_A och user_B bör nu vara utelåst från hello andra Avd schemat.</span><span class="sxs-lookup"><span data-stu-id="45310-154">With this, user_A and user_B should now be locked out from hello other dept’s schema.</span></span>
 


## <a name="next-steps"></a><span data-ttu-id="45310-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45310-155">Next steps</span></span>
<span data-ttu-id="45310-156">toolearn mer om att flytta data tooSQL datalagret finns hello [översikt över datamigrering][data migration overview].</span><span class="sxs-lookup"><span data-stu-id="45310-156">toolearn more about moving data tooSQL Data Warehouse, see hello [data migration overview][data migration overview].</span></span>

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
