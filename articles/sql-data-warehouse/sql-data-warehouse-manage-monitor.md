---
title: 'aaaMonitor din arbetsbelastning med av DMV: er | Microsoft Docs'
description: "Lär dig hur toomonitor din arbetsbelastning med av DMV: er."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 69ecd479-0941-48df-b3d0-cf54c79e6549
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: acccf952d165ccec3de3b4b1c633b18bbbf78077
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-workload-using-dmvs"></a><span data-ttu-id="533b9-103">Övervaka din arbetsbelastning med DMV:er</span><span class="sxs-lookup"><span data-stu-id="533b9-103">Monitor your workload using DMVs</span></span>
<span data-ttu-id="533b9-104">Den här artikeln beskriver hur toouse dynamiska hanteringsvyer (av DMV: er) toomonitor din arbetsbelastning och undersöka Frågekörningen i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="533b9-104">This article describes how toouse Dynamic Management Views (DMVs) toomonitor your workload and investigate query execution in Azure SQL Data Warehouse.</span></span>

## <a name="permissions"></a><span data-ttu-id="533b9-105">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="533b9-105">Permissions</span></span>
<span data-ttu-id="533b9-106">tooquery hello av DMV: er i den här artikeln behöver du antingen VISNINGSSTATUS för databasen eller behörighet.</span><span class="sxs-lookup"><span data-stu-id="533b9-106">tooquery hello DMVs in this article, you need either VIEW DATABASE STATE or CONTROL permission.</span></span> <span data-ttu-id="533b9-107">Beviljande VISNINGSSTATUS för databasen är vanligtvis hello önskad behörighet eftersom det är mycket mer restriktiva.</span><span class="sxs-lookup"><span data-stu-id="533b9-107">Usually granting VIEW DATABASE STATE is hello preferred permission as it is much more restrictive.</span></span>

```sql
GRANT VIEW DATABASE STATE toomyuser;
```

## <a name="monitor-connections"></a><span data-ttu-id="533b9-108">Övervaka anslutningar</span><span class="sxs-lookup"><span data-stu-id="533b9-108">Monitor connections</span></span>
<span data-ttu-id="533b9-109">Alla inloggningar tooSQL datalagret loggas för[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span><span class="sxs-lookup"><span data-stu-id="533b9-109">All logins tooSQL Data Warehouse are logged too[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span></span>  <span data-ttu-id="533b9-110">Den här DMV innehåller hello sista 10 000-inloggningar.</span><span class="sxs-lookup"><span data-stu-id="533b9-110">This DMV contains hello last 10,000 logins.</span></span>  <span data-ttu-id="533b9-111">Hej session_id är hello primärnyckel och har tilldelats sekventiellt för varje ny inloggning.</span><span class="sxs-lookup"><span data-stu-id="533b9-111">hello session_id is hello primary key and is assigned sequentially for each new logon.</span></span>

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a><span data-ttu-id="533b9-112">Körning av övervakaren fråga</span><span class="sxs-lookup"><span data-stu-id="533b9-112">Monitor query execution</span></span>
<span data-ttu-id="533b9-113">Alla frågor som körs på SQL Data Warehouse loggas för[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span><span class="sxs-lookup"><span data-stu-id="533b9-113">All queries executed on SQL Data Warehouse are logged too[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span></span>  <span data-ttu-id="533b9-114">Den här DMV innehåller hello sista 10 000 frågor som körs.</span><span class="sxs-lookup"><span data-stu-id="533b9-114">This DMV contains hello last 10,000 queries executed.</span></span>  <span data-ttu-id="533b9-115">Hej request_id unikt identifierar varje fråga och är hello primärnyckeln för den här DMV.</span><span class="sxs-lookup"><span data-stu-id="533b9-115">hello request_id uniquely identifies each query and is hello primary key for this DMV.</span></span>  <span data-ttu-id="533b9-116">Hej request_id tilldelas sekventiellt för varje ny fråga och med prefixet QID, vilket står för frågan-ID.</span><span class="sxs-lookup"><span data-stu-id="533b9-116">hello request_id is assigned sequentially for each new query and is prefixed with QID, which stands for query ID.</span></span>  <span data-ttu-id="533b9-117">Frågor till den här DMV för en given session_id visas alla frågor för en viss inloggning.</span><span class="sxs-lookup"><span data-stu-id="533b9-117">Querying this DMV for a given session_id shows all queries for a given logon.</span></span>

> [!NOTE]
> <span data-ttu-id="533b9-118">Lagrade procedurer använda flera begäran-ID: N.</span><span class="sxs-lookup"><span data-stu-id="533b9-118">Stored procedures use multiple Request IDs.</span></span>  <span data-ttu-id="533b9-119">Begäran-ID: N tilldelas i följd.</span><span class="sxs-lookup"><span data-stu-id="533b9-119">Request IDs are assigned in sequential order.</span></span> 
> 
> 

<span data-ttu-id="533b9-120">Här är körningen för steg toofollow tooinvestigate frågeplaner och tidpunkter för en viss fråga.</span><span class="sxs-lookup"><span data-stu-id="533b9-120">Here are steps toofollow tooinvestigate query execution plans and times for a particular query.</span></span>

### <a name="step-1-identify-hello-query-you-wish-tooinvestigate"></a><span data-ttu-id="533b9-121">STEG 1: Identifiera hello-fråga som du vill tooinvestigate</span><span class="sxs-lookup"><span data-stu-id="533b9-121">STEP 1: Identify hello query you wish tooinvestigate</span></span>
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with hello Label 'My Query'
-- Use brackets when querying hello label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

<span data-ttu-id="533b9-122">Från hello föregående resultatet och **Obs hello ID för begäran** av hello-fråga som du vill att tooinvestigate.</span><span class="sxs-lookup"><span data-stu-id="533b9-122">From hello preceding query results, **note hello Request ID** of hello query that you would like tooinvestigate.</span></span>

<span data-ttu-id="533b9-123">Frågorna i hello **pausad** tillstånd köas på grund av tooconcurrency gränser.</span><span class="sxs-lookup"><span data-stu-id="533b9-123">Queries in hello **Suspended** state are being queued due tooconcurrency limits.</span></span> <span data-ttu-id="533b9-124">De här frågorna visas också i hello sys.dm_pdw_waits väntar frågan med en typ av UserConcurrencyResourceType.</span><span class="sxs-lookup"><span data-stu-id="533b9-124">These queries also appear in hello sys.dm_pdw_waits waits query with a type of UserConcurrencyResourceType.</span></span> <span data-ttu-id="533b9-125">Se [samtidighet och arbetsbelastningen] [ Concurrency and workload management] för mer information om samtidighet gränser.</span><span class="sxs-lookup"><span data-stu-id="533b9-125">See [Concurrency and workload management][Concurrency and workload management] for more details on concurrency limits.</span></span> <span data-ttu-id="533b9-126">Frågor kan också vänta tills andra orsaker som för lås för objektet.</span><span class="sxs-lookup"><span data-stu-id="533b9-126">Queries can also wait for other reasons such as for object locks.</span></span>  <span data-ttu-id="533b9-127">Om din fråga väntar på en resurs, se [undersöker frågor som väntar på resurser] [ Investigating queries waiting for resources] längre ned i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="533b9-127">If your query is waiting for a resource, see [Investigating queries waiting for resources][Investigating queries waiting for resources] further down in this article.</span></span>

<span data-ttu-id="533b9-128">toosimplify hello sökning efter en fråga i hello sys.dm_pdw_exec_requests tabell, Använd [etikett] [ LABEL] tooassign en kommentar tooyour fråga som kan slås upp i hello sys.dm_pdw_exec_requests vy.</span><span class="sxs-lookup"><span data-stu-id="533b9-128">toosimplify hello lookup of a query in hello sys.dm_pdw_exec_requests table, use [LABEL][LABEL] tooassign a comment tooyour query that can be looked up in hello sys.dm_pdw_exec_requests view.</span></span>

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-hello-query-plan"></a><span data-ttu-id="533b9-129">STEG 2: Undersöka hello frågeplan</span><span class="sxs-lookup"><span data-stu-id="533b9-129">STEP 2: Investigate hello query plan</span></span>
<span data-ttu-id="533b9-130">Använd hello ID för begäran tooretrieve hello frågans distribuerade SQL (DSQL) plan från [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span><span class="sxs-lookup"><span data-stu-id="533b9-130">Use hello Request ID tooretrieve hello query's distributed SQL (DSQL) plan from [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span></span>

```sql
-- Find hello distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

<span data-ttu-id="533b9-131">När en plan för DSQL tar längre tid än förväntat, kan hello orsak vara en komplicerad plan med många DSQL steg eller ett enda steg som tar lång tid.</span><span class="sxs-lookup"><span data-stu-id="533b9-131">When a DSQL plan is taking longer than expected, hello cause can be a complex plan with many DSQL steps or just one step taking a long time.</span></span>  <span data-ttu-id="533b9-132">Om hello plan många steg med flera move-åtgärder kan du optimera din tabell distributioner tooreduce dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="533b9-132">If hello plan is many steps with several move operations, consider optimizing your table distributions tooreduce data movement.</span></span> <span data-ttu-id="533b9-133">Hej [tabell distribution] [ Table distribution] artikel som förklarar varför data måste vara flyttade toosolve en fråga och förklarar vissa distribution strategier toominimize dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="533b9-133">hello [Table distribution][Table distribution] article explains why data must be moved toosolve a query and explains some distribution strategies toominimize data movement.</span></span>

<span data-ttu-id="533b9-134">tooinvestigate ytterligare information om ett enda steg hello *operation_type* kolumn i hello tidskrävande frågor steg och Observera hello **steg Index**:</span><span class="sxs-lookup"><span data-stu-id="533b9-134">tooinvestigate further details about a single step, hello *operation_type* column of hello long-running query step and note hello **Step Index**:</span></span>

* <span data-ttu-id="533b9-135">Fortsätt med steg 3a för **SQL-åtgärder**: OnOperation, RemoteOperation, ReturnOperation.</span><span class="sxs-lookup"><span data-stu-id="533b9-135">Proceed with Step 3a for **SQL operations**: OnOperation, RemoteOperation, ReturnOperation.</span></span>
* <span data-ttu-id="533b9-136">Fortsätt med steg 3b för **dataflyttning operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span><span class="sxs-lookup"><span data-stu-id="533b9-136">Proceed with Step 3b for **Data Movement operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span></span>

### <a name="step-3a-investigate-sql-on-hello-distributed-databases"></a><span data-ttu-id="533b9-137">STEG 3a: undersöka SQL på hello distribuerade databaser</span><span class="sxs-lookup"><span data-stu-id="533b9-137">STEP 3a: Investigate SQL on hello distributed databases</span></span>
<span data-ttu-id="533b9-138">Använd hello ID för begäran och hello steg Index tooretrieve information från [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], som innehåller information om körning av hello frågesteg på alla hello distribuerade databaser.</span><span class="sxs-lookup"><span data-stu-id="533b9-138">Use hello Request ID and hello Step Index tooretrieve details from [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], which contains execution information of hello query step on all of hello distributed databases.</span></span>

```sql
-- Find hello distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

<span data-ttu-id="533b9-139">När du kör hello frågesteg [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan vara används tooretrieve hello SQL Server uppskattade plan från hello SQL Server plancache för hello-steget körs på en viss distribution.</span><span class="sxs-lookup"><span data-stu-id="533b9-139">When hello query step is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used tooretrieve hello SQL Server estimated plan from hello SQL Server plan cache for hello step running on a particular distribution.</span></span>

```sql
-- Find hello SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-hello-distributed-databases"></a><span data-ttu-id="533b9-140">STEG 3b: undersöka flytt av data på hello distribuerade databaser</span><span class="sxs-lookup"><span data-stu-id="533b9-140">STEP 3b: Investigate data movement on hello distributed databases</span></span>
<span data-ttu-id="533b9-141">Använd hello ID för begäran och hello steg Index tooretrieve information om ett steg för flytt av data som körs på varje distributionsplats från [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span><span class="sxs-lookup"><span data-stu-id="533b9-141">Use hello Request ID and hello Step Index tooretrieve information about a data movement step running on each distribution from [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span></span>

```sql
-- Find hello information about all hello workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* <span data-ttu-id="533b9-142">Kontrollera hello *total_elapsed_time* kolumnen toosee om en viss distributionsplats tar avsevärt längre tid än andra för dataförflyttning.</span><span class="sxs-lookup"><span data-stu-id="533b9-142">Check hello *total_elapsed_time* column toosee if a particular distribution is taking significantly longer than others for data movement.</span></span>
* <span data-ttu-id="533b9-143">Kontrollera hello för hello tidskrävande distribution *rows_processed* kolumnen toosee om hello antalet rader som flyttas från den distributionsplatsen är betydligt större än andra.</span><span class="sxs-lookup"><span data-stu-id="533b9-143">For hello long-running distribution, check hello *rows_processed* column toosee if hello number of rows being moved from that distribution is significantly larger than others.</span></span> <span data-ttu-id="533b9-144">I så fall kan detta tyda skeva underliggande data.</span><span class="sxs-lookup"><span data-stu-id="533b9-144">If so, this may indicate skew of your underlying data.</span></span>

<span data-ttu-id="533b9-145">Om hello frågan körs [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan vara används tooretrieve hello SQL Server uppskattade plan från hello SQL Server-plancache för hello körs SQL steg inom en viss distribution.</span><span class="sxs-lookup"><span data-stu-id="533b9-145">If hello query is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used tooretrieve hello SQL Server estimated plan from hello SQL Server plan cache for hello currently running SQL Step within a particular distribution.</span></span>

```sql
-- Find hello SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a><span data-ttu-id="533b9-146">Övervaka väntar frågor</span><span class="sxs-lookup"><span data-stu-id="533b9-146">Monitor waiting queries</span></span>
<span data-ttu-id="533b9-147">Om du upptäcker att frågan inte är framsteg eftersom den väntar på en resurs, är här en fråga som visar alla resurser som hello väntar på en fråga.</span><span class="sxs-lookup"><span data-stu-id="533b9-147">If you discover that your query is not making progress because it is waiting for a resource, here is a query that shows all hello resources a query is waiting for.</span></span>

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

<span data-ttu-id="533b9-148">Om frågan hello aktivt väntar på resurser från en annan fråga och sedan hello tillstånd kommer att **AcquireResources**.</span><span class="sxs-lookup"><span data-stu-id="533b9-148">If hello query is actively waiting on resources from another query, then hello state will be **AcquireResources**.</span></span>  <span data-ttu-id="533b9-149">Om hello frågan har alla hello nödvändiga resurser och sedan hello tillstånd kommer att **beviljas**.</span><span class="sxs-lookup"><span data-stu-id="533b9-149">If hello query has all hello required resources, then hello state will be **Granted**.</span></span>

## <a name="monitor-tempdb"></a><span data-ttu-id="533b9-150">Övervakaren tempdb</span><span class="sxs-lookup"><span data-stu-id="533b9-150">Monitor tempdb</span></span>
<span data-ttu-id="533b9-151">Hög tempdb-användning kan vara hello orsaken för långsam prestanda och ut ur minnesproblem.</span><span class="sxs-lookup"><span data-stu-id="533b9-151">High tempdb utilization can be hello root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="533b9-152">Kontrollera om du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder för hello först.</span><span class="sxs-lookup"><span data-stu-id="533b9-152">Please first check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="533b9-153">Överväg att skala ditt informationslager om du hittar tempdb når gränsen vid körning av fråga.</span><span class="sxs-lookup"><span data-stu-id="533b9-153">Consider scaling your data warehouse if you find tempdb reaching its limits during query execution.</span></span> <span data-ttu-id="533b9-154">hello nedan beskrivs hur tooidentify tempdb användning per fråga på varje nod.</span><span class="sxs-lookup"><span data-stu-id="533b9-154">hello following describes how tooidentify tempdb usage per query on each node.</span></span> 

<span data-ttu-id="533b9-155">Skapa hello följande vy tooassociate hello lämplig nod-id för sys.dm_pdw_sql_requests.</span><span class="sxs-lookup"><span data-stu-id="533b9-155">Create hello following view tooassociate hello appropriate node id for sys.dm_pdw_sql_requests.</span></span> <span data-ttu-id="533b9-156">Detta aktiverar du tooleverage andra direkt av DMV: er och koppla de tabellerna med sys.dm_pdw_sql_requests.</span><span class="sxs-lookup"><span data-stu-id="533b9-156">This will enable you tooleverage other pass-through DMVs and join those tables with sys.dm_pdw_sql_requests.</span></span>

```sql
-- sys.dm_pdw_sql_requests with hello correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
<span data-ttu-id="533b9-157">Kör följande fråga toomonitor tempdb hello:</span><span class="sxs-lookup"><span data-stu-id="533b9-157">Run hello following query toomonitor tempdb:</span></span>

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a><span data-ttu-id="533b9-158">Övervaka minne</span><span class="sxs-lookup"><span data-stu-id="533b9-158">Monitor memory</span></span>

<span data-ttu-id="533b9-159">Minne kan vara hello orsaken för långsam prestanda och ut ur minnesproblem.</span><span class="sxs-lookup"><span data-stu-id="533b9-159">Memory can be hello root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="533b9-160">Kontrollera om du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder för hello först.</span><span class="sxs-lookup"><span data-stu-id="533b9-160">Please first check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="533b9-161">Överväg att skala ditt informationslager om du hittar minnesanvändning för SQL Server når gränsen vid körning av fråga.</span><span class="sxs-lookup"><span data-stu-id="533b9-161">Consider scaling your data warehouse if you find SQL Server memory usage reaching its limits during query execution.</span></span>

<span data-ttu-id="533b9-162">hello följande fråga returnerar SQL Server och minnesbelastning per nod:</span><span class="sxs-lookup"><span data-stu-id="533b9-162">hello following query returns SQL Server memory usage and memory pressure per node:</span></span> 
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a><span data-ttu-id="533b9-163">Övervaka transaktion loggfilens storlek</span><span class="sxs-lookup"><span data-stu-id="533b9-163">Monitor transaction log size</span></span>
<span data-ttu-id="533b9-164">hello returnerar följande fråga hello transaktion loggfilens storlek på varje distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="533b9-164">hello following query returns hello transaction log size on each distribution.</span></span> <span data-ttu-id="533b9-165">Kontrollera om du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder för hello.</span><span class="sxs-lookup"><span data-stu-id="533b9-165">Please check if you have data skew or poor quality rowgroups and take hello appropriate actions.</span></span> <span data-ttu-id="533b9-166">Om en av hello loggfiler når 160GB, bör du skala upp din instans eller begränsa storleken på din transaktion.</span><span class="sxs-lookup"><span data-stu-id="533b9-166">If one of hello log files is reaching 160GB, you should consider scaling up your instance or limiting your transaction size.</span></span> 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
AND counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-rollback"></a><span data-ttu-id="533b9-167">Övervaka logg transaktionsåterställning</span><span class="sxs-lookup"><span data-stu-id="533b9-167">Monitor transaction log rollback</span></span>
<span data-ttu-id="533b9-168">Om dina frågor misslyckas eller tar en lång tid tooproceed kan du kontrollera och övervaka om du har några transaktioner återställs.</span><span class="sxs-lookup"><span data-stu-id="533b9-168">If your queries are failing or taking a long time tooproceed, you can check and monitor if you have any transactions rolling back.</span></span>
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a><span data-ttu-id="533b9-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="533b9-169">Next steps</span></span>
<span data-ttu-id="533b9-170">Se [systemvyer] [ System views] mer information om av DMV: er.</span><span class="sxs-lookup"><span data-stu-id="533b9-170">See [System views][System views] for more information on DMVs.</span></span>
<span data-ttu-id="533b9-171">Se [SQL Data Warehouse metodtips] [ SQL Data Warehouse best practices] för mer information om bästa praxis</span><span class="sxs-lookup"><span data-stu-id="533b9-171">See [SQL Data Warehouse best practices][SQL Data Warehouse best practices] for more information about best practices</span></span>

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
