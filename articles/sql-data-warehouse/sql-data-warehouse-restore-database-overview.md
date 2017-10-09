---
title: aaaRestore ett Azure data warehouse - lokala och geo-redundant | Microsoft Docs
description: "Översikt över hello databasen återställningsalternativ för att återställa en databas i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: 3e01c65c-6708-4fd7-82f5-4e1b5f61d304
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: a96b898372b29d420e1416ca93a172ff8af47fc7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-restore"></a><span data-ttu-id="e42e7-103">SQL Data Warehouse-återställning</span><span class="sxs-lookup"><span data-stu-id="e42e7-103">SQL Data Warehouse restore</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="e42e7-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="e42e7-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="e42e7-105">[Portal][Portal]</span><span class="sxs-lookup"><span data-stu-id="e42e7-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="e42e7-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="e42e7-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="e42e7-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="e42e7-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="e42e7-108">SQL Data Warehouse har både lokala och geografiska återställningar som en del av dess data warehouse disaster recovery-funktioner.</span><span class="sxs-lookup"><span data-stu-id="e42e7-108">SQL Data Warehouse offers both local and geographical restores as part of its data warehouse disaster recovery capabilities.</span></span> <span data-ttu-id="e42e7-109">Använd data warehouse säkerhetskopieringar toorestore data warehouse tooa återställningen peka i hello primära region eller använder geo-redundant säkerhetskopieringar toorestore tooa olika geografiska område.</span><span class="sxs-lookup"><span data-stu-id="e42e7-109">Use data warehouse backups toorestore your data warehouse tooa restore point in hello primary region, or use geo-redundant backups toorestore tooa different geographical region.</span></span> <span data-ttu-id="e42e7-110">Den här artikeln förklarar hello information om du återställer data warehouse.</span><span class="sxs-lookup"><span data-stu-id="e42e7-110">This article explains hello specifics of restoring a data warehouse.</span></span>

## <a name="what-is-a-data-warehouse-restore"></a><span data-ttu-id="e42e7-111">Vad är en återställning av data warehouse?</span><span class="sxs-lookup"><span data-stu-id="e42e7-111">What is a data warehouse restore?</span></span>
<span data-ttu-id="e42e7-112">En återställning av data warehouse är ett nytt datalager som har skapats från en säkerhetskopia av en befintlig eller borttagna data warehouse.</span><span class="sxs-lookup"><span data-stu-id="e42e7-112">A data warehouse restore is a new data warehouse that is created from a backup of an existing or deleted data warehouse.</span></span> <span data-ttu-id="e42e7-113">hello återställda data warehouse återskapar hello säkerhetskopierade data warehouse vid en viss tid.</span><span class="sxs-lookup"><span data-stu-id="e42e7-113">hello restored data warehouse re-creates hello backed-up data warehouse at a specific time.</span></span> <span data-ttu-id="e42e7-114">Eftersom SQL Data Warehouse är ett distribuerat system, skapas en återställning av data warehouse från många säkerhetskopior som lagras i Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="e42e7-114">Since SQL Data Warehouse is a distributed system, a data warehouse restore is created from many backup files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="e42e7-115">Återställning av databasen är en viktig del av alla disaster recovery strategi för affärskontinuitet och eftersom den återskapar dina data efter oavsiktliga skador eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="e42e7-115">Database restore is an essential part of any business continuity and disaster recovery strategy because it re-creates your data after accidental corruption or deletion.</span></span>

<span data-ttu-id="e42e7-116">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="e42e7-116">For more information, see:</span></span>

* [<span data-ttu-id="e42e7-117">SQL Data Warehouse-säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="e42e7-117">SQL Data Warehouse backups</span></span>](sql-data-warehouse-backups.md)
* [<span data-ttu-id="e42e7-118">Översikt över verksamhetskontinuitet</span><span class="sxs-lookup"><span data-stu-id="e42e7-118">Business continuity overview</span></span>](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a><span data-ttu-id="e42e7-119">Återställningspunkter för data warehouse</span><span class="sxs-lookup"><span data-stu-id="e42e7-119">Data warehouse restore points</span></span>
<span data-ttu-id="e42e7-120">SQL Data Warehouse använder Azure Storage Blob ögonblicksbilder toobackup hello primära data warehouse som en fördel med att använda Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="e42e7-120">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots toobackup hello primary data warehouse.</span></span> <span data-ttu-id="e42e7-121">Varje ögonblicksbild har en återställningspunkt som representerar hello tid hello ögonblicksbild som är igång.</span><span class="sxs-lookup"><span data-stu-id="e42e7-121">Each snapshot has a restore point that represents hello time hello snapshot started.</span></span> <span data-ttu-id="e42e7-122">toorestore ett datalager, som du väljer en återställningspunkt och utfärda ett kommando för återställning.</span><span class="sxs-lookup"><span data-stu-id="e42e7-122">toorestore a data warehouse, you choose a restore point and issue a restore command.</span></span>  

<span data-ttu-id="e42e7-123">SQL Data Warehouse återställs alltid hello säkerhetskopiering tooa Nytt datalager.</span><span class="sxs-lookup"><span data-stu-id="e42e7-123">SQL Data Warehouse always restores hello backup tooa new data warehouse.</span></span> <span data-ttu-id="e42e7-124">Du kan antingen behålla hello återställda data warehouse och hello aktuella eller ta bort en av dem.</span><span class="sxs-lookup"><span data-stu-id="e42e7-124">You can either keep hello restored data warehouse and hello current one, or delete one of them.</span></span> <span data-ttu-id="e42e7-125">Om du vill tooreplace hello aktuella data warehouse med hello återställts datalagret, du kan byta namn på den.</span><span class="sxs-lookup"><span data-stu-id="e42e7-125">If you want tooreplace hello current data warehouse with hello restored data warehouse, you can rename it.</span></span>

<span data-ttu-id="e42e7-126">Om du behöver toorestore borttagna eller pausas datalager, kan du [skapa ett supportärende](sql-data-warehouse-get-started-create-support-ticket.md).</span><span class="sxs-lookup"><span data-stu-id="e42e7-126">If you need toorestore a deleted or paused data warehouse, you can [create a support ticket](sql-data-warehouse-get-started-create-support-ticket.md).</span></span> 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore hello last available restore point.

Yes, for hello next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps hello data warehouse and its snapshots for seven days just in case you need hello data. After seven days, you won't be able toorestore tooany of hello restore points. -->

## <a name="geo-redundant-restore"></a><span data-ttu-id="e42e7-127">GEO-redundant återställning</span><span class="sxs-lookup"><span data-stu-id="e42e7-127">Geo-redundant restore</span></span>
<span data-ttu-id="e42e7-128">Du kan återställa din datalager tooany dataområdet stöd för Azure SQL Data Warehouse på din valda prestandanivå.</span><span class="sxs-lookup"><span data-stu-id="e42e7-128">You can restore your data warehouse tooany region supporting Azure SQL Data Warehouse at your chosen performance level.</span></span> <span data-ttu-id="e42e7-129">Observera att 9000 och 18000 DWU inte stöds i alla regioner hello förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="e42e7-129">Please note that 9000 and 18000 DWU are not supported in all regions during hello preview.</span></span>

> [!NOTE]
> <span data-ttu-id="e42e7-130">en geo-redundant tooperform återställning som du måste inte har valt att inte den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e42e7-130">tooperform a geo-redundant restore you must not have opted out of this feature.</span></span>
> 
> 

## <a name="restore-timeline"></a><span data-ttu-id="e42e7-131">Återställa tidslinjen</span><span class="sxs-lookup"><span data-stu-id="e42e7-131">Restore timeline</span></span>
<span data-ttu-id="e42e7-132">Du kan återställa en databas tooany tillgängliga återställningspunkt inom hello senaste sju dagarna.</span><span class="sxs-lookup"><span data-stu-id="e42e7-132">You can restore a database tooany available restore point within hello last seven days.</span></span> <span data-ttu-id="e42e7-133">Ögonblicksbilder Starta var fjärde timme tooeight och är tillgängliga i sju dagar.</span><span class="sxs-lookup"><span data-stu-id="e42e7-133">Snapshots start every four tooeight hours and are available for seven days.</span></span> <span data-ttu-id="e42e7-134">När en ögonblicksbild är äldre än sju dagar, den upphör att gälla och dess återställningspunkten är inte längre tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="e42e7-134">When a snapshot is older than seven days, it expires and its restore point is no longer available.</span></span>

## <a name="restore-costs"></a><span data-ttu-id="e42e7-135">Återställa kostnader</span><span class="sxs-lookup"><span data-stu-id="e42e7-135">Restore costs</span></span>
<span data-ttu-id="e42e7-136">hello lagring kostnad för hello återställda data warehouse debiteras med hello Azure Premium Storage hastighet.</span><span class="sxs-lookup"><span data-stu-id="e42e7-136">hello storage charge for hello restored data warehouse is billed at hello Azure Premium Storage rate.</span></span> 

<span data-ttu-id="e42e7-137">Om du pausar ett återställda data warehouse, debiteras du för lagring med hello Azure Premium Storage hastighet.</span><span class="sxs-lookup"><span data-stu-id="e42e7-137">If you pause a restored data warehouse, you are charged for storage at hello Azure Premium Storage rate.</span></span> <span data-ttu-id="e42e7-138">hello fördelen pausa är inte debiteras du för hello DWU datorresurser.</span><span class="sxs-lookup"><span data-stu-id="e42e7-138">hello advantage of pausing is you are not charged for hello DWU computing resources.</span></span>

<span data-ttu-id="e42e7-139">Mer information om priser för SQL Data Warehouse finns [priser för SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="e42e7-139">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="uses-for-restore"></a><span data-ttu-id="e42e7-140">Används för återställning</span><span class="sxs-lookup"><span data-stu-id="e42e7-140">Uses for restore</span></span>
<span data-ttu-id="e42e7-141">hello primär användning för återställning av data warehouse är toorecover data efter oavsiktlig dataförlust eller skadade data.</span><span class="sxs-lookup"><span data-stu-id="e42e7-141">hello primary use for data warehouse restore is toorecover data after accidental data loss or corruption.</span></span>

<span data-ttu-id="e42e7-142">Du kan också använda data warehouse återställning tooretain en säkerhetskopia för längre än sju dagar.</span><span class="sxs-lookup"><span data-stu-id="e42e7-142">You can also use data warehouse restore tooretain a backup for longer than seven days.</span></span> <span data-ttu-id="e42e7-143">När hello säkerhetskopia återställs har hello data warehouse online och kan pausa det under obestämd tid toosave beräkning kostnader.</span><span class="sxs-lookup"><span data-stu-id="e42e7-143">Once hello backup is restored, you have hello data warehouse online and can pause it indefinitely toosave compute costs.</span></span> <span data-ttu-id="e42e7-144">hello pausas databas debiteras lagring med hello Azure Premium Storage hastighet.</span><span class="sxs-lookup"><span data-stu-id="e42e7-144">hello paused database incurs storage charges at hello Azure Premium Storage rate.</span></span> 

## <a name="related-topics"></a><span data-ttu-id="e42e7-145">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="e42e7-145">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="e42e7-146">Scenarier</span><span class="sxs-lookup"><span data-stu-id="e42e7-146">Scenarios</span></span>
* <span data-ttu-id="e42e7-147">En översikt över verksamhetskontinuitet, se [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="e42e7-147">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

<span data-ttu-id="e42e7-148">tooperform ett informationslager återställa kan du återställa med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="e42e7-148">tooperform a data warehouse restore, restore using:</span></span>

* <span data-ttu-id="e42e7-149">Azure portal, se [återställa ett data warehouse med hjälp av hello Azure-portalen](sql-data-warehouse-restore-database-portal.md)</span><span class="sxs-lookup"><span data-stu-id="e42e7-149">Azure portal, see [Restore a data warehouse using hello Azure portal](sql-data-warehouse-restore-database-portal.md)</span></span>
* <span data-ttu-id="e42e7-150">PowerShell-cmdlets finns [återställa ett data warehouse med hjälp av PowerShell-cmdlets](sql-data-warehouse-restore-database-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="e42e7-150">PowerShell cmdlets, see [Restore a data warehouse using PowerShell cmdlets](sql-data-warehouse-restore-database-powershell.md)</span></span>
* <span data-ttu-id="e42e7-151">REST-API: er, se [återställa ett data warehouse med hjälp av hello REST API: er](sql-data-warehouse-restore-database-rest-api.md)</span><span class="sxs-lookup"><span data-stu-id="e42e7-151">REST APIs, see [Restore a data warehouse using hello REST APIs](sql-data-warehouse-restore-database-rest-api.md)</span></span>

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
