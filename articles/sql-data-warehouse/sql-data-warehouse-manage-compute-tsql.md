---
title: "Pausa, återuppta, skala med T-SQL i Azure SQL Data Warehouse | Microsoft Docs"
description: "Transact-SQL (T-SQL) uppgifter till skalbar prestanda genom att justera dwu: er. Spara kostnader genom att skala tillbaka under tider med låg belastning."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: a970d939-2adf-4856-8a78-d4fe8ab2cceb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/30/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 9221d72ecf8ab2ba8b04e4bc97eeef7157817cca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a><span data-ttu-id="bc282-104">Hantera datorkraft i Azure SQL Data Warehouse (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="bc282-104">Manage compute power in Azure SQL Data Warehouse (T-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bc282-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="bc282-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="bc282-106">Portal</span><span class="sxs-lookup"><span data-stu-id="bc282-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="bc282-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bc282-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="bc282-108">REST</span><span class="sxs-lookup"><span data-stu-id="bc282-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="bc282-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="bc282-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a><span data-ttu-id="bc282-110">Visa aktuella DWU-inställningar</span><span class="sxs-lookup"><span data-stu-id="bc282-110">View current DWU settings</span></span>
<span data-ttu-id="bc282-111">Visa det aktuella DWU-inställningen för dina databaser:</span><span class="sxs-lookup"><span data-stu-id="bc282-111">To view the current DWU settings for your databases:</span></span>

1. <span data-ttu-id="bc282-112">Öppna SQL Server Object Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc282-112">Open SQL Server Object Explorer in Visual Studio.</span></span>
2. <span data-ttu-id="bc282-113">Ansluta till master-databasen som är kopplade till den logiska SQL Database-servern.</span><span class="sxs-lookup"><span data-stu-id="bc282-113">Connect to the master database associated with the logical SQL Database server.</span></span>
3. <span data-ttu-id="bc282-114">Välj vyn sys.database_service_objectives dynamisk hantering.</span><span class="sxs-lookup"><span data-stu-id="bc282-114">Select from the sys.database_service_objectives dynamic management view.</span></span> <span data-ttu-id="bc282-115">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="bc282-115">Here is an example:</span></span> 

```sql
SELECT
    db.name [Database]
,   ds.edition [Edition]
,   ds.service_objective [Service Objective]
FROM
    sys.database_service_objectives ds
JOIN
    sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a><span data-ttu-id="bc282-116">Skala bearbetning</span><span class="sxs-lookup"><span data-stu-id="bc282-116">Scale compute</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="bc282-117">Ändra de dwu: er:</span><span class="sxs-lookup"><span data-stu-id="bc282-117">To change the DWUs:</span></span>

1. <span data-ttu-id="bc282-118">Ansluta till master-databasen som är associerade med din logiska SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="bc282-118">Connect to the master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="bc282-119">Använd den [ALTER DATABASE] [ ALTER DATABASE] TSQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="bc282-119">Use the [ALTER DATABASE][ALTER DATABASE] TSQL statement.</span></span> <span data-ttu-id="bc282-120">I följande exempel anger servicenivåmålet till DW1000 för MySQLDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="bc282-120">The following example sets the service level objective to DW1000 for the database MySQLDW.</span></span> 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a><span data-ttu-id="bc282-121">Kontrollera databasen tillstånd och åtgärden pågår</span><span class="sxs-lookup"><span data-stu-id="bc282-121">Check database state and operation progress</span></span>

1. <span data-ttu-id="bc282-122">Ansluta till master-databasen som är associerade med din logiska SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="bc282-122">Connect to the master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="bc282-123">Skicka fråga om du vill kontrollera databasens status</span><span class="sxs-lookup"><span data-stu-id="bc282-123">Submit query to check database state</span></span>

```sql
SELECT *
FROM
sys.databases
```

3. <span data-ttu-id="bc282-124">Skicka fråga om du vill kontrollera status för åtgärden</span><span class="sxs-lookup"><span data-stu-id="bc282-124">Submit query to check status of operation</span></span>

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

<span data-ttu-id="bc282-125">Den här DMV returnerar information om olika hanteringsåtgärder på ditt SQL Data Warehouse, till exempel igen och tillstånd för processen som ska antingen vara IN_PROGRESS eller SLUTFÖRTS.</span><span class="sxs-lookup"><span data-stu-id="bc282-125">This DMV will return information about various management operations on your SQL Data Warehouse such as the operation and the state of the operation, which will either be IN_PROGRESS or COMPLETED.</span></span>



<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="bc282-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc282-126">Next steps</span></span>
<span data-ttu-id="bc282-127">Andra hanteringsåtgärder finns [översikt över][Management overview].</span><span class="sxs-lookup"><span data-stu-id="bc282-127">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
