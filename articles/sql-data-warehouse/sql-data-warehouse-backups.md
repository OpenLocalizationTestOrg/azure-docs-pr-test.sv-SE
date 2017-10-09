---
title: "aaaAzure SQL Data Warehouse säkerhetskopieringar - ögonblicksbilder, geo-redundant | Microsoft Docs"
description: "Läs mer om SQL Data Warehouse inbyggd databassäkerhetskopieringar som gör att du toorestore en återställningspunkt för Azure SQL Data Warehouse tooa eller en annan geografisk region."
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
ms.openlocfilehash: 34659480485246f54a1490e185fc1b903fb2520d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-backups"></a><span data-ttu-id="c4296-103">SQL Data Warehouse-säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="c4296-103">SQL Data Warehouse backups</span></span>
<span data-ttu-id="c4296-104">SQL Data Warehouse erbjuder både lokala och geografiska säkerhetskopior som en del av dess data warehouse-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="c4296-104">SQL Data Warehouse offers both local and geographical backups as part of its data warehouse backup capabilities.</span></span> <span data-ttu-id="c4296-105">Dessa inkluderar Azure Storage Blob ögonblicksbilder och geo-redundant lagring.</span><span class="sxs-lookup"><span data-stu-id="c4296-105">These include Azure Storage Blob snapshots and geo-redundant storage.</span></span> <span data-ttu-id="c4296-106">Använd data warehouse säkerhetskopieringar toorestore data warehouse tooa återställningen peka i hello primära region eller återställa tooa olika geografiska region.</span><span class="sxs-lookup"><span data-stu-id="c4296-106">Use data warehouse backups toorestore your data warehouse tooa restore point in hello primary region, or restore tooa different geographical region.</span></span> <span data-ttu-id="c4296-107">Den här artikeln förklarar hello specifika säkerhetskopior i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4296-107">This article explains hello specifics of backups in SQL Data Warehouse.</span></span>

## <a name="what-is-a-data-warehouse-backup"></a><span data-ttu-id="c4296-108">Vad är en säkerhetskopiering av data warehouse?</span><span class="sxs-lookup"><span data-stu-id="c4296-108">What is a data warehouse backup?</span></span>
<span data-ttu-id="c4296-109">En säkerhetskopiering av data warehouse är hello data som du kan använda toorestore en viss tid för data warehouse tooa.</span><span class="sxs-lookup"><span data-stu-id="c4296-109">A data warehouse backup is hello data that you can use toorestore a data warehouse tooa specific time.</span></span>  <span data-ttu-id="c4296-110">Eftersom SQL Data Warehouse är ett distribuerat system, består en data warehouse-säkerhetskopia av många filer som lagras i Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="c4296-110">Since SQL Data Warehouse is a distributed system, a data warehouse backup consists of many files that are stored in Azure blobs.</span></span> 

<span data-ttu-id="c4296-111">Säkerhetskopiorna av databasen är en viktig del av alla disaster recovery strategi för affärskontinuitet och eftersom de skyddar dina data från oavsiktliga skador eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="c4296-111">Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.</span></span> <span data-ttu-id="c4296-112">Mer information finns i [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="c4296-112">For more information, see [Business continuity overview](../sql-database/sql-database-business-continuity.md).</span></span>

## <a name="data-redundancy"></a><span data-ttu-id="c4296-113">Dataredundans</span><span class="sxs-lookup"><span data-stu-id="c4296-113">Data redundancy</span></span>
<span data-ttu-id="c4296-114">SQL Data Warehouse skyddar dina data genom att lagra data i lokalt redundant (LRS) Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="c4296-114">SQL Data Warehouse protects your data by storing your data in locally redundant (LRS) Azure Premium Storage.</span></span> <span data-ttu-id="c4296-115">Funktionen Azure Storage lagrar flera synkrona kopior av hello data i hello lokala data transparent dataskydd för center tooguarantee om lokaliserade fel.</span><span class="sxs-lookup"><span data-stu-id="c4296-115">This Azure Storage feature stores multiple synchronous copies of hello data in hello local data center tooguarantee transparent data protection if there are localized failures.</span></span> <span data-ttu-id="c4296-116">Dataredundans garanterar att data kan överleva infrastruktur problem som till exempel diskfel.</span><span class="sxs-lookup"><span data-stu-id="c4296-116">Data redundancy ensures that your data can survive infrastructure issues such as disk failures.</span></span> <span data-ttu-id="c4296-117">Dataredundans säkerställer kontinuitet för företag med en feltolerant och infrastruktur med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="c4296-117">Data redundancy ensures business continuity with a fault tolerant and highly available infrastructure.</span></span>

<span data-ttu-id="c4296-118">Mer information om toolearn:</span><span class="sxs-lookup"><span data-stu-id="c4296-118">toolearn more about:</span></span>

* <span data-ttu-id="c4296-119">Azure Premium-lagring finns [introduktion tooAzure Premium-lagring](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c4296-119">Azure Premium storage, see [Introduction tooAzure Premium Storage](../storage/common/storage-premium-storage.md).</span></span>
* <span data-ttu-id="c4296-120">Lokalt Redundant lagring finns [Azure Storage-replikering](../storage/common/storage-redundancy.md#locally-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="c4296-120">Locally Redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md#locally-redundant-storage).</span></span>

## <a name="azure-storage-blob-snapshots"></a><span data-ttu-id="c4296-121">Azure Storage Blob-ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="c4296-121">Azure Storage Blob snapshots</span></span>
<span data-ttu-id="c4296-122">Som en fördel med att använda Azure Premium Storage SQL Data Warehouse använder Azure Storage Blob ögonblicksbilder toobackup hello datalagret lokalt.</span><span class="sxs-lookup"><span data-stu-id="c4296-122">As a benefit of using Azure Premium Storage, SQL Data Warehouse uses Azure Storage Blob snapshots toobackup hello data warehouse locally.</span></span> <span data-ttu-id="c4296-123">Du kan återställa en återställningspunkt för data warehouse tooa ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="c4296-123">You can restore a data warehouse tooa snapshot restore point.</span></span> <span data-ttu-id="c4296-124">Ögonblicksbilder starta minst var åttonde timme och är tillgängliga i sju dagar.</span><span class="sxs-lookup"><span data-stu-id="c4296-124">Snapshots start a minimum of every eight hours and are available for seven days.</span></span>  

<span data-ttu-id="c4296-125">Mer information om toolearn:</span><span class="sxs-lookup"><span data-stu-id="c4296-125">toolearn more about:</span></span>

* <span data-ttu-id="c4296-126">Azure blob-ögonblicksbilder finns [skapa en ögonblicksbild av blob](../storage/blobs/storage-blob-snapshots.md).</span><span class="sxs-lookup"><span data-stu-id="c4296-126">Azure blob snapshots, see [Create a blob snapshot](../storage/blobs/storage-blob-snapshots.md).</span></span>

## <a name="geo-redundant-backups"></a><span data-ttu-id="c4296-127">GEO-redundant säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="c4296-127">Geo-redundant backups</span></span>
<span data-ttu-id="c4296-128">Var 24: e timme lagrar SQL Data Warehouse hello fullständig datalagret i standardlagring.</span><span class="sxs-lookup"><span data-stu-id="c4296-128">Every 24 hours, SQL Data Warehouse stores hello full data warehouse in Standard storage.</span></span> <span data-ttu-id="c4296-129">hello fullständig datalagret skapas toomatch hello tid för senaste hello-ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="c4296-129">hello full data warehouse is created toomatch hello time of hello last snapshot.</span></span> <span data-ttu-id="c4296-130">hello standardlagring tillhör tooa geo-redundant lagringskonto med läsbehörighet (RA-GRS).</span><span class="sxs-lookup"><span data-stu-id="c4296-130">hello standard storage belongs tooa geo-redundant storage account with read access (RA-GRS).</span></span> <span data-ttu-id="c4296-131">hello Azure Storage RA-GRS funktionen replikerar hello säkerhetskopior tooa [parad Datacenter](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="c4296-131">hello Azure Storage RA-GRS feature replicates hello backup files tooa [paired data center](../best-practices-availability-paired-regions.md).</span></span> <span data-ttu-id="c4296-132">Den här georeplikering gör att du kan återställa datalagret om du inte kommer åt hello ögonblicksbilder i din primära region.</span><span class="sxs-lookup"><span data-stu-id="c4296-132">This geo-replication ensures you can restore data warehouse in case you cannot access hello snapshots in your primary region.</span></span> 

<span data-ttu-id="c4296-133">Den här funktionen är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="c4296-133">This feature is on by default.</span></span> <span data-ttu-id="c4296-134">Om du inte vill toouse geo-redundant säkerhetskopior, du kan [avanmälas] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span><span class="sxs-lookup"><span data-stu-id="c4296-134">If you don't want toouse geo-redundant backups, you can [opt out] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn).</span></span> 

> [!NOTE]
> <span data-ttu-id="c4296-135">I Azure storage hello termen *replikering* refererar toocopying filer från en plats tooanother.</span><span class="sxs-lookup"><span data-stu-id="c4296-135">In Azure storage, hello term *replication* refers toocopying files from one location tooanother.</span></span> <span data-ttu-id="c4296-136">SQL- *databasreplikering* refererar tookeeping toomultiple sekundära databaser synkroniseras med en primär databas.</span><span class="sxs-lookup"><span data-stu-id="c4296-136">SQL's *database replication* refers tookeeping toomultiple secondary databases synchronized with a primary database.</span></span> 
> 
> 

> [!NOTE]
> <span data-ttu-id="c4296-137">Du kan inte välja bort geo-redundant säkerhetskopior med DWU 9000 och DWU 18000.</span><span class="sxs-lookup"><span data-stu-id="c4296-137">You cannot opt out of geo-redundant backups with DWU 9000 and DWU 18000.</span></span> 
>
> 

<span data-ttu-id="c4296-138">Mer information om toolearn:</span><span class="sxs-lookup"><span data-stu-id="c4296-138">toolearn more about:</span></span>

* <span data-ttu-id="c4296-139">GEO-redundant lagring finns [Azure Storage-replikering](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="c4296-139">Geo-redundant storage, see [Azure Storage replication](../storage/common/storage-redundancy.md).</span></span>
* <span data-ttu-id="c4296-140">RA-GRS lagring finns [geo-redundant lagring med läsbehörighet](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="c4296-140">RA-GRS storage, see [Read-access geo-redundant storage](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).</span></span>

## <a name="data-warehouse-backup-schedule-and-retention-period"></a><span data-ttu-id="c4296-141">Schemat för säkerhetskopiering och kvarhållningsperiod datalager</span><span class="sxs-lookup"><span data-stu-id="c4296-141">Data warehouse backup schedule and retention period</span></span>
<span data-ttu-id="c4296-142">SQL Data Warehouse skapar ögonblicksbilder på dina online-datalager som alla fyra tooeight timmar och behåller varje ögonblicksbild i sju dagar.</span><span class="sxs-lookup"><span data-stu-id="c4296-142">SQL Data Warehouse creates snapshots on your online data warehouses every four tooeight hours and keeps each snapshot for seven days.</span></span> <span data-ttu-id="c4296-143">Du kan återställa din onlinedatabasen tooone hello Återställningspunkter i hello senaste sju dagarna.</span><span class="sxs-lookup"><span data-stu-id="c4296-143">You can restore your online database tooone of hello restore points in hello past seven days.</span></span> 

<span data-ttu-id="c4296-144">toosee när hello senaste ögonblicksbilden startas kör frågan på ditt online SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c4296-144">toosee when hello last snapshot started, run this query on your online SQL Data Warehouse.</span></span> 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

<span data-ttu-id="c4296-145">Om du behöver tooretain en ögonblicksbild av en längre än sju dagar kan återställa du en återställning punkt tooa Nytt datalager.</span><span class="sxs-lookup"><span data-stu-id="c4296-145">If you need tooretain a snapshot for longer than seven days, you can restore a restore point tooa new data warehouse.</span></span> <span data-ttu-id="c4296-146">När hello återställningen är klar startar SQL Data Warehouse skapar ögonblicksbilder på hello Nytt datalager.</span><span class="sxs-lookup"><span data-stu-id="c4296-146">After hello restore is finished, SQL Data Warehouse starts creating snapshots on hello new data warehouse.</span></span> <span data-ttu-id="c4296-147">Om du inte gör ändringar toohello Nytt datalager hello ögonblicksbilder förblir tom och därför hello ögonblicksbild kostnaden är minimal.</span><span class="sxs-lookup"><span data-stu-id="c4296-147">If you don't make changes toohello new data warehouse, hello snapshots stay empty and therefore hello snapshot cost is minimal.</span></span> <span data-ttu-id="c4296-148">Du kan också pausa hello databasen tookeep SQL Data Warehouse från att skapa ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="c4296-148">You could also pause hello database tookeep SQL Data Warehouse from creating snapshots.</span></span>

### <a name="what-happens-toomy-backup-retention-while-my-data-warehouse-is-paused"></a><span data-ttu-id="c4296-149">Vad händer lagring av säkerhetskopior toomy medan min datalagret pausas?</span><span class="sxs-lookup"><span data-stu-id="c4296-149">What happens toomy backup retention while my data warehouse is paused?</span></span>
<span data-ttu-id="c4296-150">SQL Data Warehouse skapar inte ögonblicksbilder och upphör inte ögonblicksbilder medan ett informationslager har pausats.</span><span class="sxs-lookup"><span data-stu-id="c4296-150">SQL Data Warehouse does not create snapshots and does not expire snapshots while a data warehouse is paused.</span></span> <span data-ttu-id="c4296-151">hello ögonblicksbild ålder ändras inte när hello-datalagret har pausats.</span><span class="sxs-lookup"><span data-stu-id="c4296-151">hello snapshot age does not change while hello data warehouse is paused.</span></span> <span data-ttu-id="c4296-152">Ögonblicksbild kvarhållning baseras på hello antalet dagar hello-datalagret är online, inte kalenderdagar.</span><span class="sxs-lookup"><span data-stu-id="c4296-152">Snapshot retention is based on hello number of days hello data warehouse is online, not calendar days.</span></span>

<span data-ttu-id="c4296-153">Om en ögonblicksbild startas 1 oktober klockan 4 och hello-datalagret är pausad 3 oktober klockan 4 är hello ögonblicksbilden två dagar gammal.</span><span class="sxs-lookup"><span data-stu-id="c4296-153">For example, if a snapshot starts October 1 at 4 pm and hello data warehouse is paused October 3 at 4 pm, hello snapshot is two days old.</span></span> <span data-ttu-id="c4296-154">När hello datalagret går tillbaka online är hello ögonblicksbilden två dagar gammal.</span><span class="sxs-lookup"><span data-stu-id="c4296-154">Whenever hello data warehouse comes back online hello snapshot is two days old.</span></span> <span data-ttu-id="c4296-155">Om hello-datalagret är online 5 oktober klockan 4 hello ögonblicksbild är två dagar gammal och förblir i fem dagar.</span><span class="sxs-lookup"><span data-stu-id="c4296-155">If hello data warehouse comes online October 5 at 4 pm, hello snapshot is two days old and remains for five more days.</span></span>

<span data-ttu-id="c4296-156">När hello-datalagret är online igen återupptar nya ögonblicksbilder SQL Data Warehouse och upphör att gälla ögonblicksbilder när de har mer än sju dagar data.</span><span class="sxs-lookup"><span data-stu-id="c4296-156">When hello data warehouse comes back online, SQL Data Warehouse resumes new snapshots and expires snapshots when they have more than seven days of data.</span></span>

### <a name="how-long-is-hello-retention-period-for-a-dropped-data-warehouse"></a><span data-ttu-id="c4296-157">Hur lång tid är hello kvarhållningsperiod för en släppt datalagret?</span><span class="sxs-lookup"><span data-stu-id="c4296-157">How long is hello retention period for a dropped data warehouse?</span></span>
<span data-ttu-id="c4296-158">När ett informationslager släpps hello datalagret och hello ögonblicksbilder sparas i sju dagar och sedan har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="c4296-158">When a data warehouse is dropped, hello data warehouse and hello snapshots are saved for seven days and then removed.</span></span> <span data-ttu-id="c4296-159">Du kan återställa hello data warehouse tooany hello spara återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="c4296-159">You can restore hello data warehouse tooany of hello saved restore points.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4296-160">Om du tar bort en logisk SQL-serverinstans tas även bort alla databaser som tillhör instansen toohello och kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="c4296-160">If you delete a logical SQL server instance, all databases that belong toohello instance are also deleted and cannot be recovered.</span></span> <span data-ttu-id="c4296-161">Du kan inte återställa en server som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="c4296-161">You cannot restore a deleted server.</span></span>
> 
> 

## <a name="data-warehouse-backup-costs"></a><span data-ttu-id="c4296-162">Data warehouse säkerhetskopiering kostnader</span><span class="sxs-lookup"><span data-stu-id="c4296-162">Data warehouse backup costs</span></span>
<span data-ttu-id="c4296-163">hello totala kostnaden för ditt primära datalagret och sju dagar i Azure Blob-ögonblicksbilder är avrundade toohello närmsta TB.</span><span class="sxs-lookup"><span data-stu-id="c4296-163">hello total cost for your primary data warehouse and seven days of Azure Blob snapshots is rounded toohello nearest TB.</span></span> <span data-ttu-id="c4296-164">Om ditt data warehouse är 1,5 TB och hello ögonblicksbilder använder 100 GB, debiteras du till exempel för 2 TB data till Azure Premium Storage-priser.</span><span class="sxs-lookup"><span data-stu-id="c4296-164">For example, if your data warehouse is 1.5 TB and hello snapshots use 100 GB, you are billed for 2 TB of data at Azure Premium Storage rates.</span></span> 

> [!NOTE]
> <span data-ttu-id="c4296-165">Varje ögonblicksbild är tom från början och växer när du gör ändringar toohello primära datalagret.</span><span class="sxs-lookup"><span data-stu-id="c4296-165">Each snapshot is empty initially and grows as you make changes toohello primary data warehouse.</span></span> <span data-ttu-id="c4296-166">Alla ögonblicksbilder ökar i storlek som hello data warehouse ändringar.</span><span class="sxs-lookup"><span data-stu-id="c4296-166">All snapshots increase in size as hello data warehouse changes.</span></span> <span data-ttu-id="c4296-167">Hello lagringskostnader för ögonblicksbilder växer därför bl.a toohello i ändringstakt.</span><span class="sxs-lookup"><span data-stu-id="c4296-167">Therefore, hello storage costs for snapshots grow according toohello rate of change.</span></span>
> 
> 

<span data-ttu-id="c4296-168">Om du använder geo-redundant lagring, får du en separat lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="c4296-168">If you are using geo-redundant storage, you receive a separate storage charge.</span></span> <span data-ttu-id="c4296-169">du debiteras hello geo-redundant lagring med hello-geografiskt Redundant lagring med läsbehörighet (RA-GRS) normalpris.</span><span class="sxs-lookup"><span data-stu-id="c4296-169">hello geo-redundant storage is billed at hello standard Read-Access Geographically Redundant Storage (RA-GRS) rate.</span></span>

<span data-ttu-id="c4296-170">Mer information om priser för SQL Data Warehouse finns [priser för SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="c4296-170">For more information about SQL Data Warehouse pricing, see [SQL Data Warehouse Pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>

## <a name="using-database-backups"></a><span data-ttu-id="c4296-171">Med hjälp av databassäkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="c4296-171">Using database backups</span></span>
<span data-ttu-id="c4296-172">hello primär användning för säkerhetskopiering av SQL data warehouse är toorestore hello data warehouse tooone hello Återställningspunkter i hello Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="c4296-172">hello primary use for SQL data warehouse backups is toorestore hello data warehouse tooone of hello restore points within hello retention period.</span></span>  

* <span data-ttu-id="c4296-173">toorestore ett SQL data warehouse finns [återställer ett SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4296-173">toorestore a SQL data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md).</span></span>

## <a name="related-topics"></a><span data-ttu-id="c4296-174">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="c4296-174">Related topics</span></span>
### <a name="scenarios"></a><span data-ttu-id="c4296-175">Scenarier</span><span class="sxs-lookup"><span data-stu-id="c4296-175">Scenarios</span></span>
* <span data-ttu-id="c4296-176">En översikt över verksamhetskontinuitet, se [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="c4296-176">For a business continuity overview, see [Business continuity overview](../sql-database/sql-database-business-continuity.md)</span></span>

<!-- ### Tasks -->

* <span data-ttu-id="c4296-177">toorestore datalager, se [återställer ett SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c4296-177">toorestore a data warehouse, see [Restore a SQL data warehouse](sql-data-warehouse-restore-database-overview.md)</span></span>

<!-- ### Tutorials -->

