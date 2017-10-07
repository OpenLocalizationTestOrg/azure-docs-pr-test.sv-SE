---
title: aaaMonitoring Azure SQL Database med dynamiska hanteringsvyer | Microsoft Docs
description: "Lär dig hur toodetect och diagnostisera vanliga prestandaproblem med hjälp av hantering av dynamiska vyer toomonitor Microsoft Azure SQL Database."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: d08f505f-3c62-47d4-bab7-35c9a834b79b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 43d5fe2dd9a38d031e9334f6ad49fce5866e3bec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a><span data-ttu-id="94217-103">Övervaka Azure SQL Database med dynamiska hanteringsvyer</span><span class="sxs-lookup"><span data-stu-id="94217-103">Monitoring Azure SQL Database using dynamic management views</span></span>
<span data-ttu-id="94217-104">Microsoft Azure SQL Database aktiverar en delmängd av hantering av dynamiska vyer toodiagnose prestandaproblem som orsakas av blockerade eller långvariga frågor, flaskhalsar för resurser, dåliga frågeplaner och så vidare.</span><span class="sxs-lookup"><span data-stu-id="94217-104">Microsoft Azure SQL Database enables a subset of dynamic management views toodiagnose performance problems, which might be caused by blocked or long-running queries, resource bottlenecks, poor query plans, and so on.</span></span> <span data-ttu-id="94217-105">Det här avsnittet innehåller information om hur toodetect vanliga prestandaproblem med dynamiska hanteringsvyer.</span><span class="sxs-lookup"><span data-stu-id="94217-105">This topic provides information on how toodetect common performance problems by using dynamic management views.</span></span>

<span data-ttu-id="94217-106">SQL Database stöder delvis tre kategorier av dynamiska hanteringsvyer:</span><span class="sxs-lookup"><span data-stu-id="94217-106">SQL Database partially supports three categories of dynamic management views:</span></span>

* <span data-ttu-id="94217-107">Databasrelaterade dynamiska hanteringsvyer.</span><span class="sxs-lookup"><span data-stu-id="94217-107">Database-related dynamic management views.</span></span>
* <span data-ttu-id="94217-108">Körningen-relaterade dynamiska hanteringsvyer.</span><span class="sxs-lookup"><span data-stu-id="94217-108">Execution-related dynamic management views.</span></span>
* <span data-ttu-id="94217-109">Relaterad dynamiska hanteringsvyer.</span><span class="sxs-lookup"><span data-stu-id="94217-109">Transaction-related dynamic management views.</span></span>

<span data-ttu-id="94217-110">Detaljerad information om dynamiska hanteringsvyer finns [vyer för hantering av dynamiska och funktioner (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) i SQL Server Books Online.</span><span class="sxs-lookup"><span data-stu-id="94217-110">For detailed information on dynamic management views, see [Dynamic Management Views and Functions (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.</span></span>

## <a name="permissions"></a><span data-ttu-id="94217-111">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="94217-111">Permissions</span></span>
<span data-ttu-id="94217-112">I SQL-databas, frågar en dynamisk hanteringsvy kräver **databasen VISNINGSSTATUS** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="94217-112">In SQL Database, querying a dynamic management view requires **VIEW DATABASE STATE** permissions.</span></span> <span data-ttu-id="94217-113">Hej **databasen VISNINGSSTATUS** behörighet returnerar information om alla objekt i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="94217-113">hello **VIEW DATABASE STATE** permission returns information about all objects within hello current database.</span></span>
<span data-ttu-id="94217-114">toogrant hello **databasen VISNINGSSTATUS** behörighet tooa specifika databasanvändare, kör följande fråga hello:</span><span class="sxs-lookup"><span data-stu-id="94217-114">toogrant hello **VIEW DATABASE STATE** permission tooa specific database user, run hello following query:</span></span>

```GRANT VIEW DATABASE STATE toodatabase_user; ```

<span data-ttu-id="94217-115">Dynamiska hanteringsvyer returnera information om tillstånd i en lokal SQL Server-instans.</span><span class="sxs-lookup"><span data-stu-id="94217-115">In an instance of on-premises SQL Server, dynamic management views return server state information.</span></span> <span data-ttu-id="94217-116">De returnera information om logiska databasen bara i SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="94217-116">In SQL Database, they return information regarding your current logical database only.</span></span>

## <a name="calculating-database-size"></a><span data-ttu-id="94217-117">Beräkning av databasens storlek</span><span class="sxs-lookup"><span data-stu-id="94217-117">Calculating database size</span></span>
<span data-ttu-id="94217-118">hello returnerar följande fråga hello storlek (i megabyte):</span><span class="sxs-lookup"><span data-stu-id="94217-118">hello following query returns hello size of your database (in megabytes):</span></span>

```
-- Calculates hello size of hello database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

<span data-ttu-id="94217-119">hello returnerar följande fråga hello storleken på enskilda objekt (i megabyte) i databasen:</span><span class="sxs-lookup"><span data-stu-id="94217-119">hello following query returns hello size of individual objects (in megabytes) in your database:</span></span>

```
-- Calculates hello size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a><span data-ttu-id="94217-120">Övervakar anslutningar</span><span class="sxs-lookup"><span data-stu-id="94217-120">Monitoring connections</span></span>
<span data-ttu-id="94217-121">Du kan använda hello [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) visa tooretrieve information om hello-anslutningar som upprättats tooa specifika Azure SQL Database-server och hello information för varje anslutning.</span><span class="sxs-lookup"><span data-stu-id="94217-121">You can use hello [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) view tooretrieve information about hello connections established tooa specific Azure SQL Database server and hello details of each connection.</span></span> <span data-ttu-id="94217-122">Dessutom hello [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) vy är användbar när du hämtar information om alla aktiva användaranslutningar och interna aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="94217-122">In addition, hello [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) view is helpful when retrieving information about all active user connections and internal tasks.</span></span>
<span data-ttu-id="94217-123">hello hämtar följande fråga information om aktuella hello-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="94217-123">hello following query retrieves information on hello current connection:</span></span>

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> <span data-ttu-id="94217-124">När du kör hello **sys.dm_exec_requests** och **sys.dm_exec_sessions vyer**, om du har **VISNINGSSTATUS för databasen** behörighet till hello databas, visas alla körs sessioner på hello-databasen. Annars kan se du endast hello aktuell session.</span><span class="sxs-lookup"><span data-stu-id="94217-124">When executing hello **sys.dm_exec_requests** and **sys.dm_exec_sessions views**, if you have **VIEW DATABASE STATE** permission on hello database, you see all executing sessions on hello database; otherwise, you see only hello current session.</span></span>
> 
> 

## <a name="monitoring-query-performance"></a><span data-ttu-id="94217-125">Övervaka prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="94217-125">Monitoring query performance</span></span>
<span data-ttu-id="94217-126">Långsamt eller länge kör frågor kan du använda betydande systemresurser.</span><span class="sxs-lookup"><span data-stu-id="94217-126">Slow or long running queries can consume significant system resources.</span></span> <span data-ttu-id="94217-127">Det här avsnittet visar hur vyer toouse dynamiska hanteringsvyn toodetect några vanliga problem med frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="94217-127">This section demonstrates how toouse dynamic management views toodetect a few common query performance problems.</span></span> <span data-ttu-id="94217-128">En äldre men ändå bra referens för felsökning, är hello [felsökning av problem med prestanda i SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) artikel på Microsoft TechNet.</span><span class="sxs-lookup"><span data-stu-id="94217-128">An older but still helpful reference for troubleshooting, is hello [Troubleshooting Performance Problems in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) article on Microsoft TechNet.</span></span>

### <a name="finding-top-n-queries"></a><span data-ttu-id="94217-129">Hitta främsta frågor</span><span class="sxs-lookup"><span data-stu-id="94217-129">Finding top N queries</span></span>
<span data-ttu-id="94217-130">hello returnerar följande exempel information om hello översta fem frågor rangordnas av Genomsnittlig CPU-tid.</span><span class="sxs-lookup"><span data-stu-id="94217-130">hello following example returns information about hello top five queries ranked by average CPU time.</span></span> <span data-ttu-id="94217-131">Det här exemplet aggregerar hello frågor enligt tootheir frågan hash, så att logiskt motsvarande frågorna grupperas efter deras kumulativa resursförbrukning.</span><span class="sxs-lookup"><span data-stu-id="94217-131">This example aggregates hello queries according tootheir query hash, so that logically equivalent queries are grouped by their cumulative resource consumption.</span></span>

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a><span data-ttu-id="94217-132">Övervaka blockerade frågor</span><span class="sxs-lookup"><span data-stu-id="94217-132">Monitoring blocked queries</span></span>
<span data-ttu-id="94217-133">Långsam eller långvariga frågor kan bidra tooexcessive resursförbrukning och hello följd av blockerade frågor.</span><span class="sxs-lookup"><span data-stu-id="94217-133">Slow or long-running queries can contribute tooexcessive resource consumption and be hello consequence of blocked queries.</span></span> <span data-ttu-id="94217-134">hello orsaken hello blockerar kan vara dålig programdesign, felaktig frågeplaner, hello bristande användbar index och så vidare.</span><span class="sxs-lookup"><span data-stu-id="94217-134">hello cause of hello blocking can be poor application design, bad query plans, hello lack of useful indexes, and so on.</span></span> <span data-ttu-id="94217-135">Du kan använda hello sys.dm_tran_locks visa tooget information om hello aktuella låsning aktiviteten i Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="94217-135">You can use hello sys.dm_tran_locks view tooget information about hello current locking activity in your Azure SQL Database.</span></span> <span data-ttu-id="94217-136">Exempelkod finns i avsnittet [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) i SQL Server Books Online.</span><span class="sxs-lookup"><span data-stu-id="94217-136">For example code, see [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.</span></span>

### <a name="monitoring-query-plans"></a><span data-ttu-id="94217-137">Övervakningsprogram för frågan</span><span class="sxs-lookup"><span data-stu-id="94217-137">Monitoring query plans</span></span>
<span data-ttu-id="94217-138">En ineffektiv frågeplan kan också öka processoranvändningen.</span><span class="sxs-lookup"><span data-stu-id="94217-138">An inefficient query plan also may increase CPU consumption.</span></span> <span data-ttu-id="94217-139">hello följande exempel används hello [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) visa toodetermine vilka frågan använder hello mest kumulativa CPU.</span><span class="sxs-lookup"><span data-stu-id="94217-139">hello following example uses hello [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) view toodetermine which query uses hello most cumulative CPU.</span></span>

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a><span data-ttu-id="94217-140">Se även</span><span class="sxs-lookup"><span data-stu-id="94217-140">See also</span></span>
[<span data-ttu-id="94217-141">Introduktion tooSQL databas</span><span class="sxs-lookup"><span data-stu-id="94217-141">Introduction tooSQL Database</span></span>](sql-database-technical-overview.md)

