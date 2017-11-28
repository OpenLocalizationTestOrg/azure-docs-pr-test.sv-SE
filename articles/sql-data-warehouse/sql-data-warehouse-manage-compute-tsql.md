---
title: "aaaPause, återuppta, skala med T-SQL i Azure SQL Data Warehouse | Microsoft Docs"
description: "Transact-SQL (T-SQL) uppgifter tooscale ut prestanda genom att justera dwu: er. Spara kostnader genom att skala tillbaka under tider med låg belastning."
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
ms.openlocfilehash: 84c6868acb673221d8853319ac9a05bb98b2b7c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a><span data-ttu-id="46d6e-104">Hantera datorkraft i Azure SQL Data Warehouse (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="46d6e-104">Manage compute power in Azure SQL Data Warehouse (T-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="46d6e-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="46d6e-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="46d6e-106">Portal</span><span class="sxs-lookup"><span data-stu-id="46d6e-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="46d6e-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="46d6e-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="46d6e-108">REST</span><span class="sxs-lookup"><span data-stu-id="46d6e-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="46d6e-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="46d6e-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a><span data-ttu-id="46d6e-110">Visa aktuella DWU-inställningar</span><span class="sxs-lookup"><span data-stu-id="46d6e-110">View current DWU settings</span></span>
<span data-ttu-id="46d6e-111">tooview hello aktuella DWU-inställningar för dina databaser:</span><span class="sxs-lookup"><span data-stu-id="46d6e-111">tooview hello current DWU settings for your databases:</span></span>

1. <span data-ttu-id="46d6e-112">Öppna SQL Server Object Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46d6e-112">Open SQL Server Object Explorer in Visual Studio.</span></span>
2. <span data-ttu-id="46d6e-113">Ansluta toohello master-databasen som är associerade med hello logiska SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="46d6e-113">Connect toohello master database associated with hello logical SQL Database server.</span></span>
3. <span data-ttu-id="46d6e-114">Välj hello sys.database_service_objectives dynamisk hanteringsvy.</span><span class="sxs-lookup"><span data-stu-id="46d6e-114">Select from hello sys.database_service_objectives dynamic management view.</span></span> <span data-ttu-id="46d6e-115">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="46d6e-115">Here is an example:</span></span> 

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

## <a name="scale-compute"></a><span data-ttu-id="46d6e-116">Skala bearbetning</span><span class="sxs-lookup"><span data-stu-id="46d6e-116">Scale compute</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="46d6e-117">toochange hello dwu: er:</span><span class="sxs-lookup"><span data-stu-id="46d6e-117">toochange hello DWUs:</span></span>

1. <span data-ttu-id="46d6e-118">Ansluta toohello master-databasen som är associerade med din logiska SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="46d6e-118">Connect toohello master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="46d6e-119">Använd hello [ALTER DATABASE] [ ALTER DATABASE] TSQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="46d6e-119">Use hello [ALTER DATABASE][ALTER DATABASE] TSQL statement.</span></span> <span data-ttu-id="46d6e-120">hello anger följande exempel hello service nivån mål tooDW1000 för hello databasen MySQLDW.</span><span class="sxs-lookup"><span data-stu-id="46d6e-120">hello following example sets hello service level objective tooDW1000 for hello database MySQLDW.</span></span> 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a><span data-ttu-id="46d6e-121">Kontrollera databasen tillstånd och åtgärden pågår</span><span class="sxs-lookup"><span data-stu-id="46d6e-121">Check database state and operation progress</span></span>

1. <span data-ttu-id="46d6e-122">Ansluta toohello master-databasen som är associerade med din logiska SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="46d6e-122">Connect toohello master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="46d6e-123">Skicka fråga toocheck databasens status</span><span class="sxs-lookup"><span data-stu-id="46d6e-123">Submit query toocheck database state</span></span>

```sql
SELECT *
FROM
sys.databases
```

3. <span data-ttu-id="46d6e-124">Skicka fråga toocheck status för åtgärden</span><span class="sxs-lookup"><span data-stu-id="46d6e-124">Submit query toocheck status of operation</span></span>

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

<span data-ttu-id="46d6e-125">Den här DMV returnerar information om olika hanteringsåtgärder på ditt SQL Data Warehouse, till exempel hello åtgärden och hello hello åtgärdens status, som ska antingen vara IN_PROGRESS eller SLUTFÖRTS.</span><span class="sxs-lookup"><span data-stu-id="46d6e-125">This DMV will return information about various management operations on your SQL Data Warehouse such as hello operation and hello state of hello operation, which will either be IN_PROGRESS or COMPLETED.</span></span>



<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="46d6e-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46d6e-126">Next steps</span></span>
<span data-ttu-id="46d6e-127">Andra hanteringsåtgärder finns [översikt över][Management overview].</span><span class="sxs-lookup"><span data-stu-id="46d6e-127">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
