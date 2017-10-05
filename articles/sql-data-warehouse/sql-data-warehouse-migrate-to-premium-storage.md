---
title: Migrera ditt befintliga Azure data warehouse till premium-lagring | Microsoft Docs
description: "Anvisningar för att migrera ett befintligt informationslager till premium-lagring"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 11/29/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 860e50b532b4b0a21d3be54f087730070b0e56bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-data-warehouse-to-premium-storage"></a><span data-ttu-id="0ccac-103">Migrera ditt data warehouse till premium-lagring</span><span class="sxs-lookup"><span data-stu-id="0ccac-103">Migrate your data warehouse to premium storage</span></span>
<span data-ttu-id="0ccac-104">Azure SQL Data Warehouse som nyligen lades [premium-lagring för bättre prestanda förutsägbarhet][premium storage for greater performance predictability].</span><span class="sxs-lookup"><span data-stu-id="0ccac-104">Azure SQL Data Warehouse recently introduced [premium storage for greater performance predictability][premium storage for greater performance predictability].</span></span> <span data-ttu-id="0ccac-105">Befintligt datalager för närvarande på standardlagring kan nu migreras till premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="0ccac-105">Existing data warehouses currently on standard storage can now be migrated to premium storage.</span></span> <span data-ttu-id="0ccac-106">Du kan dra nytta av automatisk migrering av, eller om du föredrar att reglera när du migrerar (som inbegriper vissa avbrott), kan du göra migreringen själv.</span><span class="sxs-lookup"><span data-stu-id="0ccac-106">You can take advantage of automatic migration, or if you prefer to control when to migrate (which does involve some downtime), you can do the migration yourself.</span></span>

<span data-ttu-id="0ccac-107">Om du har mer än ett datalager, använder den [schema för automatisk migrering av] [ automatic migration schedule] att avgöra när den flyttas.</span><span class="sxs-lookup"><span data-stu-id="0ccac-107">If you have more than one data warehouse, use the [automatic migration schedule][automatic migration schedule] to determine when it will also be migrated.</span></span>

## <a name="determine-storage-type"></a><span data-ttu-id="0ccac-108">Fastställa lagringstyp</span><span class="sxs-lookup"><span data-stu-id="0ccac-108">Determine storage type</span></span>
<span data-ttu-id="0ccac-109">Om du har skapat ett informationslager före följande datum använder du standardlagring.</span><span class="sxs-lookup"><span data-stu-id="0ccac-109">If you created a data warehouse before the following dates, you are currently using standard storage.</span></span>

| <span data-ttu-id="0ccac-110">**Region**</span><span class="sxs-lookup"><span data-stu-id="0ccac-110">**Region**</span></span> | <span data-ttu-id="0ccac-111">**Datalagret som skapats före detta datum**</span><span class="sxs-lookup"><span data-stu-id="0ccac-111">**Data warehouse created before this date**</span></span> |
|:--- |:--- |
| <span data-ttu-id="0ccac-112">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="0ccac-112">Australia East</span></span> |<span data-ttu-id="0ccac-113">Premium-lagring är inte tillgänglig än</span><span class="sxs-lookup"><span data-stu-id="0ccac-113">Premium storage not yet available</span></span> |
| <span data-ttu-id="0ccac-114">Östra Kina</span><span class="sxs-lookup"><span data-stu-id="0ccac-114">China East</span></span> |<span data-ttu-id="0ccac-115">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="0ccac-115">November 1, 2016</span></span> |
| <span data-ttu-id="0ccac-116">Norra Kina</span><span class="sxs-lookup"><span data-stu-id="0ccac-116">China North</span></span> |<span data-ttu-id="0ccac-117">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="0ccac-117">November 1, 2016</span></span> |
| <span data-ttu-id="0ccac-118">Centrala Tyskland</span><span class="sxs-lookup"><span data-stu-id="0ccac-118">Germany Central</span></span> |<span data-ttu-id="0ccac-119">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="0ccac-119">November 1, 2016</span></span> |
| <span data-ttu-id="0ccac-120">Nordöstra Tyskland</span><span class="sxs-lookup"><span data-stu-id="0ccac-120">Germany Northeast</span></span> |<span data-ttu-id="0ccac-121">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="0ccac-121">November 1, 2016</span></span> |
| <span data-ttu-id="0ccac-122">Västra Indien</span><span class="sxs-lookup"><span data-stu-id="0ccac-122">India West</span></span> |<span data-ttu-id="0ccac-123">Premium-lagring är inte tillgänglig än</span><span class="sxs-lookup"><span data-stu-id="0ccac-123">Premium storage not yet available</span></span> |
| <span data-ttu-id="0ccac-124">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="0ccac-124">Japan West</span></span> |<span data-ttu-id="0ccac-125">Premium-lagring är inte tillgänglig än</span><span class="sxs-lookup"><span data-stu-id="0ccac-125">Premium storage not yet available</span></span> |
| <span data-ttu-id="0ccac-126">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="0ccac-126">North Central US</span></span> |<span data-ttu-id="0ccac-127">10 november 2016</span><span class="sxs-lookup"><span data-stu-id="0ccac-127">November 10, 2016</span></span> |

## <a name="automatic-migration-details"></a><span data-ttu-id="0ccac-128">Automatisk migrering av information</span><span class="sxs-lookup"><span data-stu-id="0ccac-128">Automatic migration details</span></span>
<span data-ttu-id="0ccac-129">Som standard kommer vi att migrera din databas du mellan 6:00 och 06:00:00 i din region lokal tid under den [schema för automatisk migrering av][automatic migration schedule].</span><span class="sxs-lookup"><span data-stu-id="0ccac-129">By default, we will migrate your database for you between 6:00 PM and 6:00 AM in your region's local time during the [automatic migration schedule][automatic migration schedule].</span></span> <span data-ttu-id="0ccac-130">Ditt befintliga data warehouse kommer vara obrukbara under migreringen.</span><span class="sxs-lookup"><span data-stu-id="0ccac-130">Your existing data warehouse will be unusable during the migration.</span></span> <span data-ttu-id="0ccac-131">Migreringen tar ungefär en timme per terabyte lagring per datalagret.</span><span class="sxs-lookup"><span data-stu-id="0ccac-131">The migration will take approximately one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="0ccac-132">Du debiteras inte under delar av automatisk migrering.</span><span class="sxs-lookup"><span data-stu-id="0ccac-132">You will not be charged during any portion of the automatic migration.</span></span>

> [!NOTE]
> <span data-ttu-id="0ccac-133">När migreringen är klar att ditt data warehouse tillbaka online och kan användas.</span><span class="sxs-lookup"><span data-stu-id="0ccac-133">When the migration is complete, your data warehouse will be back online and usable.</span></span>
>
>

<span data-ttu-id="0ccac-134">Microsoft tar följande steg för att slutföra migreringen (dessa inte kräver någon inblandning från din sida).</span><span class="sxs-lookup"><span data-stu-id="0ccac-134">Microsoft is taking the following steps to complete the migration (these do not require any involvement on your part).</span></span> <span data-ttu-id="0ccac-135">I detta exempel anta att ditt befintliga data warehouse på standardlagring med namnet ”MyDW”.</span><span class="sxs-lookup"><span data-stu-id="0ccac-135">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="0ccac-136">Microsoft byter namn på ”MyDW” till ”MyDW_DO_NOT_USE_ [tidsstämpel]”.</span><span class="sxs-lookup"><span data-stu-id="0ccac-136">Microsoft renames “MyDW” to “MyDW_DO_NOT_USE_[Timestamp].”</span></span>
2. <span data-ttu-id="0ccac-137">Microsoft pausar ”MyDW_DO_NOT_USE_ [tidsstämpel]”.</span><span class="sxs-lookup"><span data-stu-id="0ccac-137">Microsoft pauses “MyDW_DO_NOT_USE_[Timestamp].”</span></span> <span data-ttu-id="0ccac-138">Under den här tiden kan görs en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="0ccac-138">During this time, a backup is taken.</span></span> <span data-ttu-id="0ccac-139">Du kan se flera pausar och återupptar om det uppstår problem under den här processen.</span><span class="sxs-lookup"><span data-stu-id="0ccac-139">You may see multiple pauses and resumes if we encounter any issues during this process.</span></span>
3. <span data-ttu-id="0ccac-140">Microsoft skapar ett nytt datalager med namnet ”MyDW” på premium-lagring från säkerhetskopior som gjorts i steg 2.</span><span class="sxs-lookup"><span data-stu-id="0ccac-140">Microsoft creates a new data warehouse named “MyDW” on premium storage from the backup taken in step 2.</span></span> <span data-ttu-id="0ccac-141">”MyDW” visas inte förrän när återställningen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="0ccac-141">“MyDW” will not appear until after the restore is complete.</span></span>
4. <span data-ttu-id="0ccac-142">När återställningen är klar ”MyDW” returnerar till samma informationslagerenheter och tillstånd (pausas eller aktiv) som den var före migreringen.</span><span class="sxs-lookup"><span data-stu-id="0ccac-142">After the restore is complete, “MyDW” returns to the same data warehouse units and state (paused or active) that it was before the migration.</span></span>
5. <span data-ttu-id="0ccac-143">När migreringen är klar, Microsoft tar bort ”MyDW_DO_NOT_USE_ [tidsstämpel]”.</span><span class="sxs-lookup"><span data-stu-id="0ccac-143">After the migration is complete, Microsoft deletes “MyDW_DO_NOT_USE_[Timestamp]”.</span></span>

> [!NOTE]
> <span data-ttu-id="0ccac-144">Följande inställningar utför inte som en del av migreringen:</span><span class="sxs-lookup"><span data-stu-id="0ccac-144">The following settings do not carry over as part of the migration:</span></span>
>
> * <span data-ttu-id="0ccac-145">Granskning på databasnivå måste återaktiveras.</span><span class="sxs-lookup"><span data-stu-id="0ccac-145">Auditing at the database level needs to be re-enabled.</span></span>
> * <span data-ttu-id="0ccac-146">Brandväggsregler på databasnivå måste läggas till på nytt.</span><span class="sxs-lookup"><span data-stu-id="0ccac-146">Firewall rules at the database level need to be re-added.</span></span> <span data-ttu-id="0ccac-147">Brandväggsregler på servernivå påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="0ccac-147">Firewall rules at the server level are not affected.</span></span>
>
>

### <a name="automatic-migration-schedule"></a><span data-ttu-id="0ccac-148">Automatisk migrering av schemat</span><span class="sxs-lookup"><span data-stu-id="0ccac-148">Automatic migration schedule</span></span>
<span data-ttu-id="0ccac-149">Automatisk migrering sker mellan 6:00 och 6:00:00 (lokal tid per region) under följande avbrott schema.</span><span class="sxs-lookup"><span data-stu-id="0ccac-149">Automatic migrations occur between 6:00 PM and 6:00 AM (local time per region) during the following outage schedule.</span></span>

| <span data-ttu-id="0ccac-150">**Region**</span><span class="sxs-lookup"><span data-stu-id="0ccac-150">**Region**</span></span> | <span data-ttu-id="0ccac-151">**Uppskattat startdatum**</span><span class="sxs-lookup"><span data-stu-id="0ccac-151">**Estimated start date**</span></span> | <span data-ttu-id="0ccac-152">**Uppskattat slutdatum**</span><span class="sxs-lookup"><span data-stu-id="0ccac-152">**Estimated end date**</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="0ccac-153">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="0ccac-153">Australia East</span></span> |<span data-ttu-id="0ccac-154">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="0ccac-154">Not determined yet</span></span> |<span data-ttu-id="0ccac-155">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="0ccac-155">Not determined yet</span></span> |
| <span data-ttu-id="0ccac-156">Östra Kina</span><span class="sxs-lookup"><span data-stu-id="0ccac-156">China East</span></span> |<span data-ttu-id="0ccac-157">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-157">January 9, 2017</span></span> |<span data-ttu-id="0ccac-158">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-158">January 13, 2017</span></span> |
| <span data-ttu-id="0ccac-159">Norra Kina</span><span class="sxs-lookup"><span data-stu-id="0ccac-159">China North</span></span> |<span data-ttu-id="0ccac-160">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-160">January 9, 2017</span></span> |<span data-ttu-id="0ccac-161">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-161">January 13, 2017</span></span> |
| <span data-ttu-id="0ccac-162">Centrala Tyskland</span><span class="sxs-lookup"><span data-stu-id="0ccac-162">Germany Central</span></span> |<span data-ttu-id="0ccac-163">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-163">January 9, 2017</span></span> |<span data-ttu-id="0ccac-164">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-164">January 13, 2017</span></span> |
| <span data-ttu-id="0ccac-165">Nordöstra Tyskland</span><span class="sxs-lookup"><span data-stu-id="0ccac-165">Germany Northeast</span></span> |<span data-ttu-id="0ccac-166">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-166">January 9, 2017</span></span> |<span data-ttu-id="0ccac-167">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-167">January 13, 2017</span></span> |
| <span data-ttu-id="0ccac-168">Västra Indien</span><span class="sxs-lookup"><span data-stu-id="0ccac-168">India West</span></span> |<span data-ttu-id="0ccac-169">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="0ccac-169">Not determined yet</span></span> |<span data-ttu-id="0ccac-170">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="0ccac-170">Not determined yet</span></span> |
| <span data-ttu-id="0ccac-171">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="0ccac-171">Japan West</span></span> |<span data-ttu-id="0ccac-172">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="0ccac-172">Not determined yet</span></span> |<span data-ttu-id="0ccac-173">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="0ccac-173">Not determined yet</span></span> |
| <span data-ttu-id="0ccac-174">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="0ccac-174">North Central US</span></span> |<span data-ttu-id="0ccac-175">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-175">January 9, 2017</span></span> |<span data-ttu-id="0ccac-176">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="0ccac-176">January 13, 2017</span></span> |

## <a name="self-migration-to-premium-storage"></a><span data-ttu-id="0ccac-177">Eget migrering till premium-lagring</span><span class="sxs-lookup"><span data-stu-id="0ccac-177">Self-migration to premium storage</span></span>
<span data-ttu-id="0ccac-178">Om du vill styra när din driftstopp uppstår, kan du använda följande steg för att migrera ett befintligt datalager på standardlagring till premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="0ccac-178">If you want to control when your downtime will occur, you can use the following steps to migrate an existing data warehouse on standard storage to premium storage.</span></span> <span data-ttu-id="0ccac-179">Om du väljer det här alternativet måste du slutföra eget migreringen innan automatisk migrering börjar i den regionen.</span><span class="sxs-lookup"><span data-stu-id="0ccac-179">If you choose this option, you must complete the self-migration before the automatic migration begins in that region.</span></span> <span data-ttu-id="0ccac-180">Detta säkerställer att du undviker risken för automatisk migrering orsakar en konflikt (avser den [schema för automatisk migrering av][automatic migration schedule]).</span><span class="sxs-lookup"><span data-stu-id="0ccac-180">This ensures that you avoid any risk of the automatic migration causing a conflict (refer to the [automatic migration schedule][automatic migration schedule]).</span></span>

### <a name="self-migration-instructions"></a><span data-ttu-id="0ccac-181">Egna migreringsanvisningar</span><span class="sxs-lookup"><span data-stu-id="0ccac-181">Self-migration instructions</span></span>
<span data-ttu-id="0ccac-182">Om du vill migrera ditt data warehouse dig själv, säkerhetskopiera och återställa funktioner.</span><span class="sxs-lookup"><span data-stu-id="0ccac-182">To migrate your data warehouse yourself, use the backup and restore features.</span></span> <span data-ttu-id="0ccac-183">Återställ del av migreringen förväntas ta ungefär en timme per terabyte lagring per datalagret.</span><span class="sxs-lookup"><span data-stu-id="0ccac-183">The restore portion of the migration is expected to take around one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="0ccac-184">Om du vill behålla samma namn när migreringen är klar, så den [steg för att byta namn på under migreringen][steps to rename during migration].</span><span class="sxs-lookup"><span data-stu-id="0ccac-184">If you want to keep the same name after migration is complete, follow the [steps to rename during migration][steps to rename during migration].</span></span>

1. <span data-ttu-id="0ccac-185">[Pausa] [ Pause] ditt data warehouse.</span><span class="sxs-lookup"><span data-stu-id="0ccac-185">[Pause][Pause] your data warehouse.</span></span> <span data-ttu-id="0ccac-186">Detta tar en automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="0ccac-186">This takes an automatic backup.</span></span>
2. <span data-ttu-id="0ccac-187">[Återställa] [ Restore] från din senaste ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="0ccac-187">[Restore][Restore] from your most recent snapshot.</span></span>
3. <span data-ttu-id="0ccac-188">Ta bort ditt befintliga data warehouse på standardlagring.</span><span class="sxs-lookup"><span data-stu-id="0ccac-188">Delete your existing data warehouse on standard storage.</span></span> <span data-ttu-id="0ccac-189">**Om du inte gör det här steget, debiteras du för båda datalager.**</span><span class="sxs-lookup"><span data-stu-id="0ccac-189">**If you fail to do this step, you will be charged for both data warehouses.**</span></span>

> [!NOTE]
> <span data-ttu-id="0ccac-190">Följande inställningar utför inte som en del av migreringen:</span><span class="sxs-lookup"><span data-stu-id="0ccac-190">The following settings do not carry over as part of the migration:</span></span>
>
> * <span data-ttu-id="0ccac-191">Granskning på databasnivå måste återaktiveras.</span><span class="sxs-lookup"><span data-stu-id="0ccac-191">Auditing at the database level needs to be re-enabled.</span></span>
> * <span data-ttu-id="0ccac-192">Brandväggsregler på databasnivå måste läggas till på nytt.</span><span class="sxs-lookup"><span data-stu-id="0ccac-192">Firewall rules at the database level need to be re-added.</span></span> <span data-ttu-id="0ccac-193">Brandväggsregler på servernivå påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="0ccac-193">Firewall rules at the server level are not affected.</span></span>
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a><span data-ttu-id="0ccac-194">Byt namn på datalagret under migreringen (valfritt)</span><span class="sxs-lookup"><span data-stu-id="0ccac-194">Rename data warehouse during migration (optional)</span></span>
<span data-ttu-id="0ccac-195">Två databaserna på samma logiska server kan inte ha samma namn.</span><span class="sxs-lookup"><span data-stu-id="0ccac-195">Two databases on the same logical server cannot have the same name.</span></span> <span data-ttu-id="0ccac-196">SQL Data Warehouse stöder nu möjligheten att byta namn på ett datalager.</span><span class="sxs-lookup"><span data-stu-id="0ccac-196">SQL Data Warehouse now supports the ability to rename a data warehouse.</span></span>

<span data-ttu-id="0ccac-197">I detta exempel anta att ditt befintliga data warehouse på standardlagring med namnet ”MyDW”.</span><span class="sxs-lookup"><span data-stu-id="0ccac-197">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="0ccac-198">Byt namn på ”MyDW” med hjälp av kommandot ALTER DATABASE.</span><span class="sxs-lookup"><span data-stu-id="0ccac-198">Rename "MyDW" by using the following ALTER DATABASE command.</span></span> <span data-ttu-id="0ccac-199">(I det här exemplet vi ska byta namn på den ”MyDW_BeforeMigration”.)  Det här kommandot stoppar alla befintliga transaktioner och måste utföras i master-databasen ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="0ccac-199">(In this example, we'll rename it "MyDW_BeforeMigration.")  This command stops all existing transactions, and must be done in the master database to succeed.</span></span>
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. <span data-ttu-id="0ccac-200">[Pausa] [ Pause] ”MyDW_BeforeMigration”.</span><span class="sxs-lookup"><span data-stu-id="0ccac-200">[Pause][Pause] "MyDW_BeforeMigration."</span></span> <span data-ttu-id="0ccac-201">Detta tar en automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="0ccac-201">This takes an automatic backup.</span></span>
3. <span data-ttu-id="0ccac-202">[Återställa] [ Restore] från din senaste ögonblicksbilden en ny databas med namnet tidigare (till exempel ”MyDW”).</span><span class="sxs-lookup"><span data-stu-id="0ccac-202">[Restore][Restore] from your most recent snapshot a new database with the name it used to be (for example, "MyDW").</span></span>
4. <span data-ttu-id="0ccac-203">Ta bort ”MyDW_BeforeMigration”.</span><span class="sxs-lookup"><span data-stu-id="0ccac-203">Delete "MyDW_BeforeMigration."</span></span> <span data-ttu-id="0ccac-204">**Om du inte gör det här steget, debiteras du för båda datalager.**</span><span class="sxs-lookup"><span data-stu-id="0ccac-204">**If you fail to do this step, you will be charged for both data warehouses.**</span></span>


## <a name="next-steps"></a><span data-ttu-id="0ccac-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ccac-205">Next steps</span></span>
<span data-ttu-id="0ccac-206">Ändra till premium-lagring ha du också ett ökat antal blob-databasfilerna i den underliggande arkitekturen för ditt informationslager.</span><span class="sxs-lookup"><span data-stu-id="0ccac-206">With the change to premium storage, you also have an increased number of database blob files in the underlying architecture of your data warehouse.</span></span> <span data-ttu-id="0ccac-207">Återskapa grupperade columnstore-index med hjälp av följande skript om du vill maximera prestandafördelarna med den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="0ccac-207">To maximize the performance benefits of this change, rebuild your clustered columnstore indexes by using the following script.</span></span> <span data-ttu-id="0ccac-208">Skriptet fungerar genom att tvinga vissa av dina befintliga data ytterligare blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ccac-208">The script works by forcing some of your existing data to the additional blobs.</span></span> <span data-ttu-id="0ccac-209">Om ingen åtgärd distribuera data naturligt över tid som du kan läsa in mer data i tabeller.</span><span class="sxs-lookup"><span data-stu-id="0ccac-209">If you take no action, the data will naturally redistribute over time as you load more data into your tables.</span></span>

<span data-ttu-id="0ccac-210">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="0ccac-210">**Prerequisites:**</span></span>

- <span data-ttu-id="0ccac-211">Datalagret ska köras med 1 000 informationslagerenheter eller högre (se [skala beräkningskraft][scale compute power]).</span><span class="sxs-lookup"><span data-stu-id="0ccac-211">The data warehouse should run with 1,000 data warehouse units or higher (see [scale compute power][scale compute power]).</span></span>
- <span data-ttu-id="0ccac-212">Användaren som kör skriptet måste vara i den [mediumrc rollen] [ mediumrc role] eller högre.</span><span class="sxs-lookup"><span data-stu-id="0ccac-212">The user executing the script should be in the [mediumrc role][mediumrc role] or higher.</span></span> <span data-ttu-id="0ccac-213">Lägg till en användare i den här rollen, kör du följande:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span><span class="sxs-lookup"><span data-stu-id="0ccac-213">To add a user to this role, execute the following: ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span></span>

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table to control index rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go

--------------------------------------------------------------------------------
-- Step 2: Execute index rebuilds. If script fails, the below can be re-run to restart where last left off.
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Clean up table created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

<span data-ttu-id="0ccac-214">Om det uppstår problem med ditt data warehouse [skapa ett supportärende] [ create a support ticket] och referens ”migreringen till premium-lagring” som möjliga orsaker.</span><span class="sxs-lookup"><span data-stu-id="0ccac-214">If you encounter any issues with your data warehouse, [create a support ticket][create a support ticket] and reference “migration to premium storage” as the possible cause.</span></span>

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps to rename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
