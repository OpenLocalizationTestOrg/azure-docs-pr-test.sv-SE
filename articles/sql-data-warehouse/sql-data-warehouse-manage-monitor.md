---
title: "Övervaka din arbetsbelastning med av DMV: er | Microsoft Docs"
description: "Lär dig att övervaka din arbetsbelastning med av DMV: er."
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
ms.openlocfilehash: 7ce6c2cdf1e28852da536414533ccdcdaeb437e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-your-workload-using-dmvs"></a><span data-ttu-id="c3e44-103">Övervaka din arbetsbelastning med DMV:er</span><span class="sxs-lookup"><span data-stu-id="c3e44-103">Monitor your workload using DMVs</span></span>
<span data-ttu-id="c3e44-104">Den här artikeln beskriver hur du använder dynamiska hanteringsvyer (av DMV: er) för att övervaka din arbetsbelastning och undersöka Frågekörningen i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c3e44-104">This article describes how to use Dynamic Management Views (DMVs) to monitor your workload and investigate query execution in Azure SQL Data Warehouse.</span></span>

## <a name="permissions"></a><span data-ttu-id="c3e44-105">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="c3e44-105">Permissions</span></span>
<span data-ttu-id="c3e44-106">Om du vill fråga de av DMV: er i den här artikeln behöver behörighet att visa status för databasen eller kontroll.</span><span class="sxs-lookup"><span data-stu-id="c3e44-106">To query the DMVs in this article, you need either VIEW DATABASE STATE or CONTROL permission.</span></span> <span data-ttu-id="c3e44-107">Beviljande VISNINGSSTATUS för databasen är vanligtvis prioriterade behörigheten eftersom den är mycket mer restriktiva.</span><span class="sxs-lookup"><span data-stu-id="c3e44-107">Usually granting VIEW DATABASE STATE is the preferred permission as it is much more restrictive.</span></span>

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a><span data-ttu-id="c3e44-108">Övervaka anslutningar</span><span class="sxs-lookup"><span data-stu-id="c3e44-108">Monitor connections</span></span>
<span data-ttu-id="c3e44-109">Alla inloggningar till SQL Data Warehouse loggas [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span><span class="sxs-lookup"><span data-stu-id="c3e44-109">All logins to SQL Data Warehouse are logged to [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].</span></span>  <span data-ttu-id="c3e44-110">Den här DMV innehåller de senaste 10 000-inloggningar.</span><span class="sxs-lookup"><span data-stu-id="c3e44-110">This DMV contains the last 10,000 logins.</span></span>  <span data-ttu-id="c3e44-111">Session_id är den primära nyckeln och har tilldelats sekventiellt för varje ny inloggning.</span><span class="sxs-lookup"><span data-stu-id="c3e44-111">The session_id is the primary key and is assigned sequentially for each new logon.</span></span>

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a><span data-ttu-id="c3e44-112">Körning av övervakaren fråga</span><span class="sxs-lookup"><span data-stu-id="c3e44-112">Monitor query execution</span></span>
<span data-ttu-id="c3e44-113">Alla frågor som körs på SQL Data Warehouse loggas [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span><span class="sxs-lookup"><span data-stu-id="c3e44-113">All queries executed on SQL Data Warehouse are logged to [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].</span></span>  <span data-ttu-id="c3e44-114">Den här DMV innehåller de senaste 10 000 frågor som körs.</span><span class="sxs-lookup"><span data-stu-id="c3e44-114">This DMV contains the last 10,000 queries executed.</span></span>  <span data-ttu-id="c3e44-115">Request_id unikt identifierar varje fråga och är den primära nyckeln för den här DMV.</span><span class="sxs-lookup"><span data-stu-id="c3e44-115">The request_id uniquely identifies each query and is the primary key for this DMV.</span></span>  <span data-ttu-id="c3e44-116">Request_id tilldelas sekventiellt för varje ny fråga och med prefixet QID, vilket står för frågan-ID.</span><span class="sxs-lookup"><span data-stu-id="c3e44-116">The request_id is assigned sequentially for each new query and is prefixed with QID, which stands for query ID.</span></span>  <span data-ttu-id="c3e44-117">Frågor till den här DMV för en given session_id visas alla frågor för en viss inloggning.</span><span class="sxs-lookup"><span data-stu-id="c3e44-117">Querying this DMV for a given session_id shows all queries for a given logon.</span></span>

> [!NOTE]
> <span data-ttu-id="c3e44-118">Lagrade procedurer använda flera begäran-ID: N.</span><span class="sxs-lookup"><span data-stu-id="c3e44-118">Stored procedures use multiple Request IDs.</span></span>  <span data-ttu-id="c3e44-119">Begäran-ID: N tilldelas i följd.</span><span class="sxs-lookup"><span data-stu-id="c3e44-119">Request IDs are assigned in sequential order.</span></span> 
> 
> 

<span data-ttu-id="c3e44-120">Här följer stegen för att undersöka frågeplaner för körning och tid för en viss fråga.</span><span class="sxs-lookup"><span data-stu-id="c3e44-120">Here are steps to follow to investigate query execution plans and times for a particular query.</span></span>

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a><span data-ttu-id="c3e44-121">STEG 1: Identifiera den fråga som du vill undersöka</span><span class="sxs-lookup"><span data-stu-id="c3e44-121">STEP 1: Identify the query you wish to investigate</span></span>
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

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

<span data-ttu-id="c3e44-122">Från föregående frågeresultaten **Observera begäran-ID** till den fråga som du vill undersöka.</span><span class="sxs-lookup"><span data-stu-id="c3e44-122">From the preceding query results, **note the Request ID** of the query that you would like to investigate.</span></span>

<span data-ttu-id="c3e44-123">Frågor i den **pausad** tillstånd köas på grund av samtidiga gränser.</span><span class="sxs-lookup"><span data-stu-id="c3e44-123">Queries in the **Suspended** state are being queued due to concurrency limits.</span></span> <span data-ttu-id="c3e44-124">De här frågorna visas också i sys.dm_pdw_waits väntar frågan med en typ av UserConcurrencyResourceType.</span><span class="sxs-lookup"><span data-stu-id="c3e44-124">These queries also appear in the sys.dm_pdw_waits waits query with a type of UserConcurrencyResourceType.</span></span> <span data-ttu-id="c3e44-125">Se [samtidighet och arbetsbelastningen] [ Concurrency and workload management] för mer information om samtidighet gränser.</span><span class="sxs-lookup"><span data-stu-id="c3e44-125">See [Concurrency and workload management][Concurrency and workload management] for more details on concurrency limits.</span></span> <span data-ttu-id="c3e44-126">Frågor kan också vänta tills andra orsaker som för lås för objektet.</span><span class="sxs-lookup"><span data-stu-id="c3e44-126">Queries can also wait for other reasons such as for object locks.</span></span>  <span data-ttu-id="c3e44-127">Om din fråga väntar på en resurs, se [undersöker frågor som väntar på resurser] [ Investigating queries waiting for resources] längre ned i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c3e44-127">If your query is waiting for a resource, see [Investigating queries waiting for resources][Investigating queries waiting for resources] further down in this article.</span></span>

<span data-ttu-id="c3e44-128">För att förenkla sökning efter en fråga i tabellen sys.dm_pdw_exec_requests, Använd [etikett] [ LABEL] att tilldela en kommentar till din fråga kan sökas i vyn sys.dm_pdw_exec_requests.</span><span class="sxs-lookup"><span data-stu-id="c3e44-128">To simplify the lookup of a query in the sys.dm_pdw_exec_requests table, use [LABEL][LABEL] to assign a comment to your query that can be looked up in the sys.dm_pdw_exec_requests view.</span></span>

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a><span data-ttu-id="c3e44-129">STEG 2: Undersöka frågeplanen</span><span class="sxs-lookup"><span data-stu-id="c3e44-129">STEP 2: Investigate the query plan</span></span>
<span data-ttu-id="c3e44-130">Använda begäran-ID för att hämta frågans distribuerade SQL (DSQL) plan från [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span><span class="sxs-lookup"><span data-stu-id="c3e44-130">Use the Request ID to retrieve the query's distributed SQL (DSQL) plan from [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].</span></span>

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

<span data-ttu-id="c3e44-131">När en plan för DSQL tar längre tid än förväntat, kan orsaken vara en komplicerad plan med många DSQL steg eller ett enda steg som tar lång tid.</span><span class="sxs-lookup"><span data-stu-id="c3e44-131">When a DSQL plan is taking longer than expected, the cause can be a complex plan with many DSQL steps or just one step taking a long time.</span></span>  <span data-ttu-id="c3e44-132">Om planen många steg med flera move-åtgärder kan du optimera din tabell distributioner för att minska dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="c3e44-132">If the plan is many steps with several move operations, consider optimizing your table distributions to reduce data movement.</span></span> <span data-ttu-id="c3e44-133">Den [tabell distribution] [ Table distribution] artikel som förklarar varför data måste flyttas till att lösa en fråga och förklarar vissa distributionsstrategier för att minimera dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="c3e44-133">The [Table distribution][Table distribution] article explains why data must be moved to solve a query and explains some distribution strategies to minimize data movement.</span></span>

<span data-ttu-id="c3e44-134">Att undersöka ytterligare information om ett enskilt steg i *operation_type* kolumnen tidskrävande frågesteg och den **steg Index**:</span><span class="sxs-lookup"><span data-stu-id="c3e44-134">To investigate further details about a single step, the *operation_type* column of the long-running query step and note the **Step Index**:</span></span>

* <span data-ttu-id="c3e44-135">Fortsätt med steg 3a för **SQL-åtgärder**: OnOperation, RemoteOperation, ReturnOperation.</span><span class="sxs-lookup"><span data-stu-id="c3e44-135">Proceed with Step 3a for **SQL operations**: OnOperation, RemoteOperation, ReturnOperation.</span></span>
* <span data-ttu-id="c3e44-136">Fortsätt med steg 3b för **dataflyttning operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span><span class="sxs-lookup"><span data-stu-id="c3e44-136">Proceed with Step 3b for **Data Movement operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.</span></span>

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a><span data-ttu-id="c3e44-137">STEG 3a: undersöka SQL på de distribuerade databaserna</span><span class="sxs-lookup"><span data-stu-id="c3e44-137">STEP 3a: Investigate SQL on the distributed databases</span></span>
<span data-ttu-id="c3e44-138">Använda begäran-ID och indexet steg för att hämta information från [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], som innehåller information om körningen av steget fråga på alla distribuerade databaser.</span><span class="sxs-lookup"><span data-stu-id="c3e44-138">Use the Request ID and the Step Index to retrieve details from [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], which contains execution information of the query step on all of the distributed databases.</span></span>

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

<span data-ttu-id="c3e44-139">När du kör steget frågan [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan användas för att hämta den uppskattade planen för SQL Server från SQL Server-plancache för steget körs på en viss distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="c3e44-139">When the query step is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used to retrieve the SQL Server estimated plan from the SQL Server plan cache for the step running on a particular distribution.</span></span>

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a><span data-ttu-id="c3e44-140">STEG 3b: undersöka flytt av data på de distribuerade databaserna</span><span class="sxs-lookup"><span data-stu-id="c3e44-140">STEP 3b: Investigate data movement on the distributed databases</span></span>
<span data-ttu-id="c3e44-141">Använda begäran-ID och indexet steg för att hämta information om ett steg för flytt av data som körs på varje distributionsplats från [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span><span class="sxs-lookup"><span data-stu-id="c3e44-141">Use the Request ID and the Step Index to retrieve information about a data movement step running on each distribution from [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].</span></span>

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* <span data-ttu-id="c3e44-142">Kontrollera den *total_elapsed_time* kolumnen att se om en viss distributionsplats tar betydligt längre än andra för dataförflyttning.</span><span class="sxs-lookup"><span data-stu-id="c3e44-142">Check the *total_elapsed_time* column to see if a particular distribution is taking significantly longer than others for data movement.</span></span>
* <span data-ttu-id="c3e44-143">Långvariga-distribution Kontrollera den *rows_processed* kolumn om antalet rader som flyttas från den distributionsplatsen är betydligt större än andra.</span><span class="sxs-lookup"><span data-stu-id="c3e44-143">For the long-running distribution, check the *rows_processed* column to see if the number of rows being moved from that distribution is significantly larger than others.</span></span> <span data-ttu-id="c3e44-144">I så fall kan detta tyda skeva underliggande data.</span><span class="sxs-lookup"><span data-stu-id="c3e44-144">If so, this may indicate skew of your underlying data.</span></span>

<span data-ttu-id="c3e44-145">Om frågan körs [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan användas för att hämta den uppskattade planen för SQL Server från SQL Server-plancache för pågående SQL steg inom en viss distribution.</span><span class="sxs-lookup"><span data-stu-id="c3e44-145">If the query is running, [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] can be used to retrieve the SQL Server estimated plan from the SQL Server plan cache for the currently running SQL Step within a particular distribution.</span></span>

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a><span data-ttu-id="c3e44-146">Övervaka väntar frågor</span><span class="sxs-lookup"><span data-stu-id="c3e44-146">Monitor waiting queries</span></span>
<span data-ttu-id="c3e44-147">Om du upptäcker att frågan inte är framsteg eftersom den väntar på en resurs, är här en fråga som visar alla resurser som väntar på en fråga.</span><span class="sxs-lookup"><span data-stu-id="c3e44-147">If you discover that your query is not making progress because it is waiting for a resource, here is a query that shows all the resources a query is waiting for.</span></span>

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

<span data-ttu-id="c3e44-148">Om frågan är aktivt väntar på resurser från en annan fråga, så kommer att vara i tillståndet **AcquireResources**.</span><span class="sxs-lookup"><span data-stu-id="c3e44-148">If the query is actively waiting on resources from another query, then the state will be **AcquireResources**.</span></span>  <span data-ttu-id="c3e44-149">Om frågan har alla nödvändiga resurser och tillståndet kommer att **beviljas**.</span><span class="sxs-lookup"><span data-stu-id="c3e44-149">If the query has all the required resources, then the state will be **Granted**.</span></span>

## <a name="monitor-tempdb"></a><span data-ttu-id="c3e44-150">Övervakaren tempdb</span><span class="sxs-lookup"><span data-stu-id="c3e44-150">Monitor tempdb</span></span>
<span data-ttu-id="c3e44-151">Hög tempdb-användning kan vara orsaken för långsam prestanda och ut ur minnesproblem.</span><span class="sxs-lookup"><span data-stu-id="c3e44-151">High tempdb utilization can be the root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="c3e44-152">Kontrollera först att du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c3e44-152">Please first check if you have data skew or poor quality rowgroups and take the appropriate actions.</span></span> <span data-ttu-id="c3e44-153">Överväg att skala ditt informationslager om du hittar tempdb når gränsen vid körning av fråga.</span><span class="sxs-lookup"><span data-stu-id="c3e44-153">Consider scaling your data warehouse if you find tempdb reaching its limits during query execution.</span></span> <span data-ttu-id="c3e44-154">Nedan beskrivs hur du identifierar tempdb-användning per fråga på varje nod.</span><span class="sxs-lookup"><span data-stu-id="c3e44-154">The following describes how to identify tempdb usage per query on each node.</span></span> 

<span data-ttu-id="c3e44-155">Skapa följande vy om du vill associera lämpliga nod-id för sys.dm_pdw_sql_requests.</span><span class="sxs-lookup"><span data-stu-id="c3e44-155">Create the following view to associate the appropriate node id for sys.dm_pdw_sql_requests.</span></span> <span data-ttu-id="c3e44-156">Detta kan du dra nytta av andra direkt av DMV: er och koppla de tabellerna med sys.dm_pdw_sql_requests.</span><span class="sxs-lookup"><span data-stu-id="c3e44-156">This will enable you to leverage other pass-through DMVs and join those tables with sys.dm_pdw_sql_requests.</span></span>

```sql
-- sys.dm_pdw_sql_requests with the correct node id
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
<span data-ttu-id="c3e44-157">Kör följande fråga för att övervaka tempdb:</span><span class="sxs-lookup"><span data-stu-id="c3e44-157">Run the following query to monitor tempdb:</span></span>

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
## <a name="monitor-memory"></a><span data-ttu-id="c3e44-158">Övervaka minne</span><span class="sxs-lookup"><span data-stu-id="c3e44-158">Monitor memory</span></span>

<span data-ttu-id="c3e44-159">Minne kan vara orsaken för långsam prestanda och ut ur minnesproblem.</span><span class="sxs-lookup"><span data-stu-id="c3e44-159">Memory can be the root cause for slow performance and out of memory issues.</span></span> <span data-ttu-id="c3e44-160">Kontrollera först att du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c3e44-160">Please first check if you have data skew or poor quality rowgroups and take the appropriate actions.</span></span> <span data-ttu-id="c3e44-161">Överväg att skala ditt informationslager om du hittar minnesanvändning för SQL Server når gränsen vid körning av fråga.</span><span class="sxs-lookup"><span data-stu-id="c3e44-161">Consider scaling your data warehouse if you find SQL Server memory usage reaching its limits during query execution.</span></span>

<span data-ttu-id="c3e44-162">Följande fråga returnerar SQL Server och minnesbelastning per nod:</span><span class="sxs-lookup"><span data-stu-id="c3e44-162">The following query returns SQL Server memory usage and memory pressure per node:</span></span>   
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
## <a name="monitor-transaction-log-size"></a><span data-ttu-id="c3e44-163">Övervaka transaktion loggfilens storlek</span><span class="sxs-lookup"><span data-stu-id="c3e44-163">Monitor transaction log size</span></span>
<span data-ttu-id="c3e44-164">Följande fråga returnerar storleken på transaktionsloggen på varje distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="c3e44-164">The following query returns the transaction log size on each distribution.</span></span> <span data-ttu-id="c3e44-165">Kontrollera om du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c3e44-165">Please check if you have data skew or poor quality rowgroups and take the appropriate actions.</span></span> <span data-ttu-id="c3e44-166">Om en av filerna når 160GB, bör du skala upp din instans eller begränsa storleken på din transaktion.</span><span class="sxs-lookup"><span data-stu-id="c3e44-166">If one of the log files is reaching 160GB, you should consider scaling up your instance or limiting your transaction size.</span></span> 
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
## <a name="monitor-transaction-log-rollback"></a><span data-ttu-id="c3e44-167">Övervaka logg transaktionsåterställning</span><span class="sxs-lookup"><span data-stu-id="c3e44-167">Monitor transaction log rollback</span></span>
<span data-ttu-id="c3e44-168">Om dina frågor misslyckas eller tar lång tid att fortsätta, kan du kontrollera och övervaka om du har några transaktioner återställs.</span><span class="sxs-lookup"><span data-stu-id="c3e44-168">If your queries are failing or taking a long time to proceed, you can check and monitor if you have any transactions rolling back.</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="c3e44-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3e44-169">Next steps</span></span>
<span data-ttu-id="c3e44-170">Se [systemvyer] [ System views] mer information om av DMV: er.</span><span class="sxs-lookup"><span data-stu-id="c3e44-170">See [System views][System views] for more information on DMVs.</span></span>
<span data-ttu-id="c3e44-171">Se [SQL Data Warehouse metodtips] [ SQL Data Warehouse best practices] för mer information om bästa praxis</span><span class="sxs-lookup"><span data-stu-id="c3e44-171">See [SQL Data Warehouse best practices][SQL Data Warehouse best practices] for more information about best practices</span></span>

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
