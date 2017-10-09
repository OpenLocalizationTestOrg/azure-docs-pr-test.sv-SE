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
# <a name="monitor-your-workload-using-dmvs"></a>Övervaka din arbetsbelastning med DMV:er
Den här artikeln beskriver hur toouse dynamiska hanteringsvyer (av DMV: er) toomonitor din arbetsbelastning och undersöka Frågekörningen i Azure SQL Data Warehouse.

## <a name="permissions"></a>Behörigheter
tooquery hello av DMV: er i den här artikeln behöver du antingen VISNINGSSTATUS för databasen eller behörighet. Beviljande VISNINGSSTATUS för databasen är vanligtvis hello önskad behörighet eftersom det är mycket mer restriktiva.

```sql
GRANT VIEW DATABASE STATE toomyuser;
```

## <a name="monitor-connections"></a>Övervaka anslutningar
Alla inloggningar tooSQL datalagret loggas för[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  Den här DMV innehåller hello sista 10 000-inloggningar.  Hej session_id är hello primärnyckel och har tilldelats sekventiellt för varje ny inloggning.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Körning av övervakaren fråga
Alla frågor som körs på SQL Data Warehouse loggas för[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  Den här DMV innehåller hello sista 10 000 frågor som körs.  Hej request_id unikt identifierar varje fråga och är hello primärnyckeln för den här DMV.  Hej request_id tilldelas sekventiellt för varje ny fråga och med prefixet QID, vilket står för frågan-ID.  Frågor till den här DMV för en given session_id visas alla frågor för en viss inloggning.

> [!NOTE]
> Lagrade procedurer använda flera begäran-ID: N.  Begäran-ID: N tilldelas i följd. 
> 
> 

Här är körningen för steg toofollow tooinvestigate frågeplaner och tidpunkter för en viss fråga.

### <a name="step-1-identify-hello-query-you-wish-tooinvestigate"></a>STEG 1: Identifiera hello-fråga som du vill tooinvestigate
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

Från hello föregående resultatet och **Obs hello ID för begäran** av hello-fråga som du vill att tooinvestigate.

Frågorna i hello **pausad** tillstånd köas på grund av tooconcurrency gränser. De här frågorna visas också i hello sys.dm_pdw_waits väntar frågan med en typ av UserConcurrencyResourceType. Se [samtidighet och arbetsbelastningen] [ Concurrency and workload management] för mer information om samtidighet gränser. Frågor kan också vänta tills andra orsaker som för lås för objektet.  Om din fråga väntar på en resurs, se [undersöker frågor som väntar på resurser] [ Investigating queries waiting for resources] längre ned i den här artikeln.

toosimplify hello sökning efter en fråga i hello sys.dm_pdw_exec_requests tabell, Använd [etikett] [ LABEL] tooassign en kommentar tooyour fråga som kan slås upp i hello sys.dm_pdw_exec_requests vy.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-hello-query-plan"></a>STEG 2: Undersöka hello frågeplan
Använd hello ID för begäran tooretrieve hello frågans distribuerade SQL (DSQL) plan från [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find hello distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

När en plan för DSQL tar längre tid än förväntat, kan hello orsak vara en komplicerad plan med många DSQL steg eller ett enda steg som tar lång tid.  Om hello plan många steg med flera move-åtgärder kan du optimera din tabell distributioner tooreduce dataflyttning. Hej [tabell distribution] [ Table distribution] artikel som förklarar varför data måste vara flyttade toosolve en fråga och förklarar vissa distribution strategier toominimize dataflyttning.

tooinvestigate ytterligare information om ett enda steg hello *operation_type* kolumn i hello tidskrävande frågor steg och Observera hello **steg Index**:

* Fortsätt med steg 3a för **SQL-åtgärder**: OnOperation, RemoteOperation, ReturnOperation.
* Fortsätt med steg 3b för **dataflyttning operations**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-hello-distributed-databases"></a>STEG 3a: undersöka SQL på hello distribuerade databaser
Använd hello ID för begäran och hello steg Index tooretrieve information från [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], som innehåller information om körning av hello frågesteg på alla hello distribuerade databaser.

```sql
-- Find hello distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

När du kör hello frågesteg [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan vara används tooretrieve hello SQL Server uppskattade plan från hello SQL Server plancache för hello-steget körs på en viss distribution.

```sql
-- Find hello SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-hello-distributed-databases"></a>STEG 3b: undersöka flytt av data på hello distribuerade databaser
Använd hello ID för begäran och hello steg Index tooretrieve information om ett steg för flytt av data som körs på varje distributionsplats från [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find hello information about all hello workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Kontrollera hello *total_elapsed_time* kolumnen toosee om en viss distributionsplats tar avsevärt längre tid än andra för dataförflyttning.
* Kontrollera hello för hello tidskrävande distribution *rows_processed* kolumnen toosee om hello antalet rader som flyttas från den distributionsplatsen är betydligt större än andra. I så fall kan detta tyda skeva underliggande data.

Om hello frågan körs [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] kan vara används tooretrieve hello SQL Server uppskattade plan från hello SQL Server-plancache för hello körs SQL steg inom en viss distribution.

```sql
-- Find hello SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>Övervaka väntar frågor
Om du upptäcker att frågan inte är framsteg eftersom den väntar på en resurs, är här en fråga som visar alla resurser som hello väntar på en fråga.

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

Om frågan hello aktivt väntar på resurser från en annan fråga och sedan hello tillstånd kommer att **AcquireResources**.  Om hello frågan har alla hello nödvändiga resurser och sedan hello tillstånd kommer att **beviljas**.

## <a name="monitor-tempdb"></a>Övervakaren tempdb
Hög tempdb-användning kan vara hello orsaken för långsam prestanda och ut ur minnesproblem. Kontrollera om du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder för hello först. Överväg att skala ditt informationslager om du hittar tempdb når gränsen vid körning av fråga. hello nedan beskrivs hur tooidentify tempdb användning per fråga på varje nod. 

Skapa hello följande vy tooassociate hello lämplig nod-id för sys.dm_pdw_sql_requests. Detta aktiverar du tooleverage andra direkt av DMV: er och koppla de tabellerna med sys.dm_pdw_sql_requests.

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
Kör följande fråga toomonitor tempdb hello:

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
## <a name="monitor-memory"></a>Övervaka minne

Minne kan vara hello orsaken för långsam prestanda och ut ur minnesproblem. Kontrollera om du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder för hello först. Överväg att skala ditt informationslager om du hittar minnesanvändning för SQL Server når gränsen vid körning av fråga.

hello följande fråga returnerar SQL Server och minnesbelastning per nod: 
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
## <a name="monitor-transaction-log-size"></a>Övervaka transaktion loggfilens storlek
hello returnerar följande fråga hello transaktion loggfilens storlek på varje distributionsplats. Kontrollera om du har data skeva eller dålig rowgroups och vidta lämpliga åtgärder för hello. Om en av hello loggfiler når 160GB, bör du skala upp din instans eller begränsa storleken på din transaktion. 
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
## <a name="monitor-transaction-log-rollback"></a>Övervaka logg transaktionsåterställning
Om dina frågor misslyckas eller tar en lång tid tooproceed kan du kontrollera och övervaka om du har några transaktioner återställs.
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

## <a name="next-steps"></a>Nästa steg
Se [systemvyer] [ System views] mer information om av DMV: er.
Se [SQL Data Warehouse metodtips] [ SQL Data Warehouse best practices] för mer information om bästa praxis

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
