---
title: "Säkerhetskopieringar i Azure SQL Data Warehouse - ögonblicksbilder, geo-redundant | Microsoft Docs"
description: "Mer information om SQL Data Warehouse inbyggd databassäkerhetskopieringar som gör att du kan återställa en Azure SQL Data Warehouse till en återställningspunkt eller en annan geografisk region."
services: sql-data-warehouse
documentationcenter: 
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 54c0149a769e654139bbdf709802d49127f041ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="sql-data-warehouse-backups"></a><span data-ttu-id="43419-103">SQL Data Warehouse-säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="43419-103">SQL Data Warehouse backups</span></span>
<span data-ttu-id="43419-104">SQL Data Warehouse erbjuder både lokala och geografiska säkerhetskopior som en del av dess data warehouse-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="43419-104">SQL Data Warehouse offers both local and geographical backups as part of its data warehouse backup capabilities.</span></span> <span data-ttu-id="43419-105">Dessa inkluderar Azure Storage Blob ögonblicksbilder och geo-redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="43419-105">These include Azure Storage Blob snapshots and geo-redundant storage.</span></span> <span data-ttu-id="43419-106">Använd data warehouse säkerhetskopieringar för att återställa ditt datalager till en återställningspunkt i den primära regionen eller Återställ till en annan geografisk region.</span><span class="sxs-lookup"><span data-stu-id="43419-106">Use data warehouse backups to restore your data warehouse to a restore point in the primary region, or restore to a different geographical region.</span></span> <span data-ttu-id="43419-107">Den här artikeln beskriver egenskaperna för säkerhetskopiering i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="43419-107">This article explains the specifics of backups in SQL Data Warehouse.</span></span>

## <a name="what-is-a-data-warehouse-backup"></a><span data-ttu-id="43419-108">Vad är en säkerhetskopiering av data warehouse?</span><span class="sxs-lookup"><span data-stu-id="43419-108">What is a data warehouse backup?</span></span>
<span data-ttu-id="43419-109">En säkerhetskopiering av data warehouse är data som du kan använda för att återställa ett datalager till en viss tid.</span><span class="sxs-lookup"><span data-stu-id="43419-109">A data warehouse backup is the data that you can use to restore a data warehouse to a specific time.</span></span>  <span data-ttu-id="43419-110">Eftersom SQL Data Warehouse är ett distribuerat system, består en data warehouse-säkerhetskopia av många filer som lagras i Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="43419-110">Since SQL Data Warehouse is a distributed system, a data warehouse backup consists of many files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="43419-111">Säkerhetskopiorna av databasen är en viktig del av alla disaster recovery strategi för affärskontinuitet och eftersom de skyddar dina data från oavsiktliga skador eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="43419-111">Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.</span></span> <span data-ttu-id="43419-112">Mer information finns i [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="43419-112">For more information, see [Business continuity overview](../sql-database/sql-database-business-continuity.md).</span></span>

## <a name="data-redundancy"></a><span data-ttu-id="43419-113">Dataredundans</span><span class="sxs-lookup"><span data-stu-id="43419-113">Data redundancy</span></span>
<span data-ttu-id="43419-114">SQL Data Warehouse skyddar dina data genom att lagra data i lokalt redundant (LRS) Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="43419-114">SQL Data Warehouse protects your data by storing your data in locally redundant (LRS) Azure Premium Storage.</span></span> <span data-ttu-id="43419-115">Funktionen Azure Storage lagrar flera synkrona kopior av data i det lokala datacentret för att garantera transparent dataskydd om lokaliserade fel.</span><span class="sxs-lookup"><span data-stu-id="43419-115">This Azure Storage feature stores multiple synchronous copies of the data in the local data center to guarantee transparent data protection if there are localized failures.</span></span> <span data-ttu-id="43419-116">Dataredundans garanterar att data kan överleva infrastruktur problem som till exempel diskfel.</span><span class="sxs-lookup"><span data-stu-id="43419-116">Data redundancy ensures that your data can survive infrastructure issues such as disk failures.</span></span> <span data-ttu-id="43419-117">Dataredundans säkerställer kontinuitet för företag med en feltolerant och infrastruktur med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="43419-117">Data redundancy ensures business continuity with a fault tolerant and highly available infrastructure.</span></span>

<span data-ttu-id="43419-118">Mer information om:</span><span class="sxs-lookup"><span data-stu-id="43419-118">To learn more about:</span></span>

* <span data-ttu-id="43419-119">Azure Premium-lagring finns [introduktion till Azure Premium Storage](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="43419-119">Azure Premium storage, see [Introduction to Azure Premium Storage](../storage/common/storage-premium-storage.md).</span></span>
* <span data-ttu-id="43419-120">Lokalt Redundant lagring finns [Azure Storage-replikering](../storage/common/storage-redundancy.md#locally-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="43419-120">Locally Redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md#locally-redundant-storage).</span></span>

## <a name="azure-storage-blob-snapshots"></a><span data-ttu-id="43419-121">Azure Storage Blob-ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="43419-121">Azure Storage Blob snapshots</span></span>
<span data-ttu-id="43419-122">SQL Data Warehouse använder Azure Storage Blob ögonblicksbilder för att säkerhetskopiera data warehouse lokalt som en fördel med att använda Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="43419-122">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots to backup the data warehouse locally.</span></span> <span data-ttu-id="43419-123">Du kan återställa ett datalager till en ögonblicksbild återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="43419-123">You can restore a data warehouse to a snapshot restore point.</span></span> <span data-ttu-id="43419-124">Ögonblicksbilder starta minst var åttonde timme och är tillgängliga i sju dagar.</span><span class="sxs-lookup"><span data-stu-id="43419-124">Snapshots start a minimum of every eight hours and are available for seven days.</span></span>  

<span data-ttu-id="43419-125">Mer information om:</span><span class="sxs-lookup"><span data-stu-id="43419-125">To learn more about:</span></span>

* <span data-ttu-id="43419-126">Azure blob-ögonblicksbilder finns [skapa en ögonblicksbild av blob](../storage/blobs/storage-blob-snapshots.md).</span><span class="sxs-lookup"><span data-stu-id="43419-126">Azure blob snapshots, see [Create a blob snapshot](../storage/blobs/storage-blob-snapshots.md).</span></span>

## <a name="geo-redundant-backups"></a><span data-ttu-id="43419-127">GEO-redundant säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="43419-127">Geo-redundant backups</span></span>
<span data-ttu-id="43419-128">Var 24: e timme lagrar SQL Data Warehouse fullständig datalagret i standardlagring.</span><span class="sxs-lookup"><span data-stu-id="43419-128">Every 24 hours, SQL Data Warehouse stores the full data warehouse in Standard storage.</span></span> <span data-ttu-id="43419-129">Fullständig datalagret skapas så att den matchar tiden för den senaste ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="43419-129">The full data warehouse is created to match the time of the last snapshot.</span></span> <span data-ttu-id="43419-130">Standardlagring hör till ett konto för geo-redundant lagring med läsbehörighet (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="43419-130">The standard storage belongs to a geo-redundant storage account with read access (RA-GRS).</span></span> <span data-ttu-id="43419-131">Funktionen Azure Storage RA-GRS replikeras de säkerhetskopiera filerna till en [parad Datacenter](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="43419-131">The Azure Storage RA-GRS feature replicates the backup files to a [paired data center](../best-practices-availability-paired-regions.md).</span></span> <span data-ttu-id="43419-132">Den här georeplikering gör att du kan återställa datalagret om du inte kommer åt ögonblicksbilder i din primära region.</span><span class="sxs-lookup"><span data-stu-id="43419-132">This geo-replication ensures you can restore data warehouse in case you cannot access the snapshots in your primary region.</span></span> 

<span data-ttu-id="43419-133">Den här funktionen är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="43419-133">This feature is on by default.</span></span> <span data-ttu-id="43419-134">Om du inte vill använda geo-redundant säkerhetskopior du kan [avanmälas] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span><span class="sxs-lookup"><span data-stu-id="43419-134">If you don't want to use geo-redundant backups, you can [opt out] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span></span> 

> [!NOTE]
> <span data-ttu-id="43419-135">I Azure storage termen *replikering* refererar till kopiera filer från en plats till en annan.</span><span class="sxs-lookup"><span data-stu-id="43419-135">In Azure storage, the term *replication* refers to copying files from one location to another.</span></span> <span data-ttu-id="43419-136">SQL- *databasreplikering* refererar till att hålla till flera sekundära databaser som synkroniseras med en primär databas.</span><span class="sxs-lookup"><span data-stu-id="43419-136">SQL's *database replication* refers to keeping to multiple secondary databases synchronized with a primary database.</span></span> 
> 
> 

> [!NOTE]
> <span data-ttu-id="43419-137">Du kan inte välja bort geo-redundant säkerhetskopior med DWU 9000 och DWU 18000.</span><span class="sxs-lookup"><span data-stu-id="43419-137">You cannot opt out of geo-redundant backups with DWU 9000 and DWU 18000.</span></span> 
>
> 

<span data-ttu-id="43419-138">Mer information om:</span><span class="sxs-lookup"><span data-stu-id="43419-138">To learn more about:</span></span>

* <span data-ttu-id="43419-139">GEO-redundant lagring finns [Azure Storage-replikering](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="43419-139">Geo-redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md).</span></span>
* <span data-ttu-id="43419-140">RA-GRS lagring finns [geo-redundant lagring med läsbehörighet](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="43419-140">RA-GRS storage, see [Read-access geo-redundant storage](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span></span>

## <a name="data-warehouse-backup-schedule-and-retention-period"></a><span data-ttu-id="43419-141">Schemat för säkerhetskopiering och kvarhållningsperiod datalager</span><span class="sxs-lookup"><span data-stu-id="43419-141">Data warehouse backup schedule and retention period</span></span>
<span data-ttu-id="43419-142">SQL Data Warehouse skapar ögonblicksbilder på dina online-datalager som alla fyra till åtta timmar och fortsätter varje ögonblicksbild i sju dagar.</span><span class="sxs-lookup"><span data-stu-id="43419-142">SQL Data Warehouse creates snapshots on your online data warehouses every four to eight hours and keeps each snapshot for seven days.</span></span> <span data-ttu-id="43419-143">Du kan återställa databasen online till en återställningspunkter under de senaste sju dagarna.</span><span class="sxs-lookup"><span data-stu-id="43419-143">You can restore your online database to one of the restore points in the past seven days.</span></span> 

<span data-ttu-id="43419-144">Om du vill se när den senaste ögonblicksbilden startas, kör du den här frågan på ditt online SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="43419-144">To see when the last snapshot started, run this query on your online SQL Data Warehouse.</span></span> 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

<span data-ttu-id="43419-145">Om du vill behålla en ögonblicksbild för längre än sju dagar kan du återställa en återställningspunkt till ett nytt datalager.</span><span class="sxs-lookup"><span data-stu-id="43419-145">If you need to retain a snapshot for longer than seven days, you can restore a restore point to a new data warehouse.</span></span> <span data-ttu-id="43419-146">När återställningen är klar startar SQL Data Warehouse att skapa ögonblicksbilder på nytt datalager.</span><span class="sxs-lookup"><span data-stu-id="43419-146">After the restore is finished, SQL Data Warehouse starts creating snapshots on the new data warehouse.</span></span> <span data-ttu-id="43419-147">Om du inte ändrar Nytt datalager ögonblicksbilderna förblir tom och därför ögonblicksbild kostnaden är minimal.</span><span class="sxs-lookup"><span data-stu-id="43419-147">If you don't make changes to the new data warehouse, the snapshots stay empty and therefore the snapshot cost is minimal.</span></span> <span data-ttu-id="43419-148">Du kan också pausa databas om du vill behålla SQL Data Warehouse från att skapa ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="43419-148">You could also pause the database to keep SQL Data Warehouse from creating snapshots.</span></span>

### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a><span data-ttu-id="43419-149">Vad händer med min lagring av säkerhetskopior medan min datalagret pausas?</span><span class="sxs-lookup"><span data-stu-id="43419-149">What happens to my backup retention while my data warehouse is paused?</span></span>
<span data-ttu-id="43419-150">SQL Data Warehouse skapar inte ögonblicksbilder och upphör inte ögonblicksbilder medan ett informationslager har pausats.</span><span class="sxs-lookup"><span data-stu-id="43419-150">SQL Data Warehouse does not create snapshots and does not expire snapshots while a data warehouse is paused.</span></span> <span data-ttu-id="43419-151">En ögonblicksbild ålder ändras inte när datalagret har pausats.</span><span class="sxs-lookup"><span data-stu-id="43419-151">The snapshot age does not change while the data warehouse is paused.</span></span> <span data-ttu-id="43419-152">Ögonblicksbild kvarhållning baseras på hur många dagar data warehouse är online, inte kalenderdagar.</span><span class="sxs-lookup"><span data-stu-id="43419-152">Snapshot retention is based on the number of days the data warehouse is online, not calendar days.</span></span>

<span data-ttu-id="43419-153">Om en ögonblicksbild startas 1 oktober klockan 4 och datalagret har pausats 3 oktober klockan 4, är ögonblicksbilden två dagar gammal.</span><span class="sxs-lookup"><span data-stu-id="43419-153">For example, if a snapshot starts October 1 at 4 pm and the data warehouse is paused October 3 at 4 pm, the snapshot is two days old.</span></span> <span data-ttu-id="43419-154">När datalagret går tillbaka online är ögonblicksbilden två dagar gammal.</span><span class="sxs-lookup"><span data-stu-id="43419-154">Whenever the data warehouse comes back online the snapshot is two days old.</span></span> <span data-ttu-id="43419-155">Om datalagret är online 5 oktober klockan 4 ögonblicksbilden är två dagar gammal och förblir i fem dagar.</span><span class="sxs-lookup"><span data-stu-id="43419-155">If the data warehouse comes online October 5 at 4 pm, the snapshot is two days old and remains for five more days.</span></span>

<span data-ttu-id="43419-156">När datalagret går tillbaka online SQL Data Warehouse återupptar nya ögonblicksbilder och upphör att gälla ögonblicksbilder när de har mer än sju dagar data.</span><span class="sxs-lookup"><span data-stu-id="43419-156">When the data warehouse comes back online, SQL Data Warehouse resumes new snapshots and expires snapshots when they have more than seven days of data.</span></span>

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a><span data-ttu-id="43419-157">Hur lång tid är kvarhållningsperiod för en släppt datalagret?</span><span class="sxs-lookup"><span data-stu-id="43419-157">How long is the retention period for a dropped data warehouse?</span></span>
<span data-ttu-id="43419-158">När ett informationslager släpps, datalagret och ögonblicksbilderna sparas i sju dagar och sedan har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="43419-158">When a data warehouse is dropped, the data warehouse and the snapshots are saved for seven days and then removed.</span></span> <span data-ttu-id="43419-159">Du kan återställa data warehouse till någon av de sparade återställningspunkterna.</span><span class="sxs-lookup"><span data-stu-id="43419-159">You can restore the data warehouse to any of the saved restore points.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="43419-160">Om du tar bort en logisk SQL server-instansen tas även bort alla databaser som tillhör instansen och kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="43419-160">If you delete a logical SQL server instance, all databases that belong to the instance are also deleted and cannot be recovered.</span></span> <span data-ttu-id="43419-161">Du kan inte återställa en server som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="43419-161">You cannot restore a deleted server.</span></span>
> 
> 

## <a name="data-warehouse-backup-costs"></a><span data-ttu-id="43419-162">Data warehouse säkerhetskopiering kostnader</span><span class="sxs-lookup"><span data-stu-id="43419-162">Data warehouse backup costs</span></span>
<span data-ttu-id="43419-163">Den totala kostnaden för ditt primära datalagret och sju dagar i Azure Blob-ögonblicksbilder avrundas till närmaste TB.</span><span class="sxs-lookup"><span data-stu-id="43419-163">The total cost for your primary data warehouse and seven days of Azure Blob snapshots is rounded to the nearest TB.</span></span> <span data-ttu-id="43419-164">Om ditt data warehouse är 1,5 TB och ögonblicksbilderna använder 100 GB, debiteras du till exempel för 2 TB data till Azure Premium Storage-priser.</span><span class="sxs-lookup"><span data-stu-id="43419-164">For example, if your data warehouse is 1.5 TB and the snapshots use 100 GB, you are billed for 2 TB of data at Azure Premium Storage rates.</span></span> 

> [!NOTE]
> <span data-ttu-id="43419-165">Varje ögonblicksbild är tom från början och växer när du gör ändringar i den primära datalagret.</span><span class="sxs-lookup"><span data-stu-id="43419-165">Each snapshot is empty initially and grows as you make changes to the primary data warehouse.</span></span> <span data-ttu-id="43419-166">Alla ögonblicksbilder ökar i storlek när data warehouse ändras.</span><span class="sxs-lookup"><span data-stu-id="43419-166">All snapshots increase in size as the data warehouse changes.</span></span> <span data-ttu-id="43419-167">Lagringskostnader för ögonblicksbilder växer därför enligt ändringshastigheten.</span><span class="sxs-lookup"><span data-stu-id="43419-167">Therefore, the storage costs for snapshots grow according to the rate of change.</span></span>
> 
> 

<span data-ttu-id="43419-168">Om du använder geo-redundant lagring, får du en separat lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="43419-168">If you are using geo-redundant storage, you receive a separate storage charge.</span></span> <span data-ttu-id="43419-169">Geo-redundant lagring faktureras enligt standardkostnaden geografiskt Redundant lagring med läsbehörighet (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="43419-169">The geo-redundant storage is billed at the standard Read-Access Geographically Redundant Storage (RA-GRS) rate.</span></span>

<span data-ttu-id="43419-170">Mer information om priser för SQL Data Warehouse finns [priser för SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="43419-170">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="using-database-backups"></a><span data-ttu-id="43419-171">Med hjälp av databassäkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="43419-171">Using database backups</span></span>
<span data-ttu-id="43419-172">Primär användning för säkerhetskopiering av SQL data warehouse är att återställa datalagret till en återställningspunkter inom kvarhållningsperioden.</span><span class="sxs-lookup"><span data-stu-id="43419-172">The primary use for SQL data warehouse backups is to restore the data warehouse to one of the restore points within the retention period.</span></span>  

* <span data-ttu-id="43419-173">Om du vill återställa en SQL data warehouse finns [återställer ett SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span><span class="sxs-lookup"><span data-stu-id="43419-173">To restore a SQL data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span></span>

## <a name="related-topics"></a><span data-ttu-id="43419-174">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="43419-174">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="43419-175">Scenarier</span><span class="sxs-lookup"><span data-stu-id="43419-175">Scenarios</span></span>
* <span data-ttu-id="43419-176">En översikt över verksamhetskontinuitet, se [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="43419-176">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

* <span data-ttu-id="43419-177">Om du vill återställa ett informationslager finns [återställer ett SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span><span class="sxs-lookup"><span data-stu-id="43419-177">To restore a data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span></span>

<!-- ### Tutorials -->

