---
title: aaaMigrate dina befintliga Azure data warehouse toopremium storage | Microsoft Docs
description: "Anvisningar för att migrera en befintlig data warehouse toopremium lagring"
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
ms.openlocfilehash: 145199c2da1f6f1fb8898626cd04886c42d82204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data-warehouse-toopremium-storage"></a><span data-ttu-id="1859c-103">Migrera dina data warehouse toopremium lagring</span><span class="sxs-lookup"><span data-stu-id="1859c-103">Migrate your data warehouse toopremium storage</span></span>
<span data-ttu-id="1859c-104">Azure SQL Data Warehouse som nyligen lades [premium-lagring för bättre prestanda förutsägbarhet][premium storage for greater performance predictability].</span><span class="sxs-lookup"><span data-stu-id="1859c-104">Azure SQL Data Warehouse recently introduced [premium storage for greater performance predictability][premium storage for greater performance predictability].</span></span> <span data-ttu-id="1859c-105">Befintliga data distributionslager för närvarande på standardlagring ska migreras toopremium lagring.</span><span class="sxs-lookup"><span data-stu-id="1859c-105">Existing data warehouses currently on standard storage can now be migrated toopremium storage.</span></span> <span data-ttu-id="1859c-106">Du kan dra nytta av automatisk migrering eller om du föredrar toocontrol när toomigrate (som inbegriper vissa avbrott), kan du göra hello migrering själv.</span><span class="sxs-lookup"><span data-stu-id="1859c-106">You can take advantage of automatic migration, or if you prefer toocontrol when toomigrate (which does involve some downtime), you can do hello migration yourself.</span></span>

<span data-ttu-id="1859c-107">Om du har mer än ett datalager, använder hello [schema för automatisk migrering av] [ automatic migration schedule] toodetermine när den flyttas.</span><span class="sxs-lookup"><span data-stu-id="1859c-107">If you have more than one data warehouse, use hello [automatic migration schedule][automatic migration schedule] toodetermine when it will also be migrated.</span></span>

## <a name="determine-storage-type"></a><span data-ttu-id="1859c-108">Fastställa lagringstyp</span><span class="sxs-lookup"><span data-stu-id="1859c-108">Determine storage type</span></span>
<span data-ttu-id="1859c-109">Om du har skapat ett informationslager innan hello efter datum använder du standardlagring.</span><span class="sxs-lookup"><span data-stu-id="1859c-109">If you created a data warehouse before hello following dates, you are currently using standard storage.</span></span>

| <span data-ttu-id="1859c-110">**Region**</span><span class="sxs-lookup"><span data-stu-id="1859c-110">**Region**</span></span> | <span data-ttu-id="1859c-111">**Datalagret som skapats före detta datum**</span><span class="sxs-lookup"><span data-stu-id="1859c-111">**Data warehouse created before this date**</span></span> |
|:--- |:--- |
| <span data-ttu-id="1859c-112">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="1859c-112">Australia East</span></span> |<span data-ttu-id="1859c-113">Premium-lagring är inte tillgänglig än</span><span class="sxs-lookup"><span data-stu-id="1859c-113">Premium storage not yet available</span></span> |
| <span data-ttu-id="1859c-114">Östra Kina</span><span class="sxs-lookup"><span data-stu-id="1859c-114">China East</span></span> |<span data-ttu-id="1859c-115">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="1859c-115">November 1, 2016</span></span> |
| <span data-ttu-id="1859c-116">Norra Kina</span><span class="sxs-lookup"><span data-stu-id="1859c-116">China North</span></span> |<span data-ttu-id="1859c-117">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="1859c-117">November 1, 2016</span></span> |
| <span data-ttu-id="1859c-118">Centrala Tyskland</span><span class="sxs-lookup"><span data-stu-id="1859c-118">Germany Central</span></span> |<span data-ttu-id="1859c-119">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="1859c-119">November 1, 2016</span></span> |
| <span data-ttu-id="1859c-120">Nordöstra Tyskland</span><span class="sxs-lookup"><span data-stu-id="1859c-120">Germany Northeast</span></span> |<span data-ttu-id="1859c-121">Den 1 november 2016</span><span class="sxs-lookup"><span data-stu-id="1859c-121">November 1, 2016</span></span> |
| <span data-ttu-id="1859c-122">Västra Indien</span><span class="sxs-lookup"><span data-stu-id="1859c-122">India West</span></span> |<span data-ttu-id="1859c-123">Premium-lagring är inte tillgänglig än</span><span class="sxs-lookup"><span data-stu-id="1859c-123">Premium storage not yet available</span></span> |
| <span data-ttu-id="1859c-124">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="1859c-124">Japan West</span></span> |<span data-ttu-id="1859c-125">Premium-lagring är inte tillgänglig än</span><span class="sxs-lookup"><span data-stu-id="1859c-125">Premium storage not yet available</span></span> |
| <span data-ttu-id="1859c-126">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="1859c-126">North Central US</span></span> |<span data-ttu-id="1859c-127">10 november 2016</span><span class="sxs-lookup"><span data-stu-id="1859c-127">November 10, 2016</span></span> |

## <a name="automatic-migration-details"></a><span data-ttu-id="1859c-128">Automatisk migrering av information</span><span class="sxs-lookup"><span data-stu-id="1859c-128">Automatic migration details</span></span>
<span data-ttu-id="1859c-129">Som standard kommer vi att migrera din databas du mellan 6:00 och 06:00:00 i din region lokal tid under hello [schema för automatisk migrering av][automatic migration schedule].</span><span class="sxs-lookup"><span data-stu-id="1859c-129">By default, we will migrate your database for you between 6:00 PM and 6:00 AM in your region's local time during hello [automatic migration schedule][automatic migration schedule].</span></span> <span data-ttu-id="1859c-130">Ditt befintliga data warehouse är oanvändbar under hello migreringen.</span><span class="sxs-lookup"><span data-stu-id="1859c-130">Your existing data warehouse will be unusable during hello migration.</span></span> <span data-ttu-id="1859c-131">hello migreringen tar ungefär en timme per terabyte lagring per datalagret.</span><span class="sxs-lookup"><span data-stu-id="1859c-131">hello migration will take approximately one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="1859c-132">Du debiteras inte under delar av hello automatisk migrering.</span><span class="sxs-lookup"><span data-stu-id="1859c-132">You will not be charged during any portion of hello automatic migration.</span></span>

> [!NOTE]
> <span data-ttu-id="1859c-133">När hello migreringen är klar att ditt data warehouse tillbaka online och kan användas.</span><span class="sxs-lookup"><span data-stu-id="1859c-133">When hello migration is complete, your data warehouse will be back online and usable.</span></span>
>
>

<span data-ttu-id="1859c-134">Microsoft tar hello följande steg toocomplete hello migrering (dessa inte kräver någon inblandning från din sida).</span><span class="sxs-lookup"><span data-stu-id="1859c-134">Microsoft is taking hello following steps toocomplete hello migration (these do not require any involvement on your part).</span></span> <span data-ttu-id="1859c-135">I detta exempel anta att ditt befintliga data warehouse på standardlagring med namnet ”MyDW”.</span><span class="sxs-lookup"><span data-stu-id="1859c-135">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="1859c-136">Microsoft byter namn på ”MyDW” för ”MyDW_DO_NOT_USE_ [tidsstämpel]”.</span><span class="sxs-lookup"><span data-stu-id="1859c-136">Microsoft renames “MyDW” too“MyDW_DO_NOT_USE_[Timestamp].”</span></span>
2. <span data-ttu-id="1859c-137">Microsoft pausar ”MyDW_DO_NOT_USE_ [tidsstämpel]”.</span><span class="sxs-lookup"><span data-stu-id="1859c-137">Microsoft pauses “MyDW_DO_NOT_USE_[Timestamp].”</span></span> <span data-ttu-id="1859c-138">Under den här tiden kan görs en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="1859c-138">During this time, a backup is taken.</span></span> <span data-ttu-id="1859c-139">Du kan se flera pausar och återupptar om det uppstår problem under den här processen.</span><span class="sxs-lookup"><span data-stu-id="1859c-139">You may see multiple pauses and resumes if we encounter any issues during this process.</span></span>
3. <span data-ttu-id="1859c-140">Microsoft skapar ett nytt datalager med namnet ”MyDW” på premium-lagring från hello säkerhetskopia i steg 2.</span><span class="sxs-lookup"><span data-stu-id="1859c-140">Microsoft creates a new data warehouse named “MyDW” on premium storage from hello backup taken in step 2.</span></span> <span data-ttu-id="1859c-141">”MyDW” visas inte förrän efter hello återställningen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="1859c-141">“MyDW” will not appear until after hello restore is complete.</span></span>
4. <span data-ttu-id="1859c-142">När hello återställningen är klar ”MyDW” returnerar toohello samma data warehouse enheter och -läget (pausas eller aktiv) som den var innan hello migreringen.</span><span class="sxs-lookup"><span data-stu-id="1859c-142">After hello restore is complete, “MyDW” returns toohello same data warehouse units and state (paused or active) that it was before hello migration.</span></span>
5. <span data-ttu-id="1859c-143">När hello migreringen är klar, Microsoft tar bort ”MyDW_DO_NOT_USE_ [tidsstämpel]”.</span><span class="sxs-lookup"><span data-stu-id="1859c-143">After hello migration is complete, Microsoft deletes “MyDW_DO_NOT_USE_[Timestamp]”.</span></span>

> [!NOTE]
> <span data-ttu-id="1859c-144">hello utför följande inställningar inte som en del av hello migrering:</span><span class="sxs-lookup"><span data-stu-id="1859c-144">hello following settings do not carry over as part of hello migration:</span></span>
>
> * <span data-ttu-id="1859c-145">Granskning på databasnivå hello måste toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="1859c-145">Auditing at hello database level needs toobe re-enabled.</span></span>
> * <span data-ttu-id="1859c-146">Brandväggsregler på databasnivå hello måste toobe som lagts till igen.</span><span class="sxs-lookup"><span data-stu-id="1859c-146">Firewall rules at hello database level need toobe re-added.</span></span> <span data-ttu-id="1859c-147">Brandväggsregler på servernivå hello påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="1859c-147">Firewall rules at hello server level are not affected.</span></span>
>
>

### <a name="automatic-migration-schedule"></a><span data-ttu-id="1859c-148">Automatisk migrering av schemat</span><span class="sxs-lookup"><span data-stu-id="1859c-148">Automatic migration schedule</span></span>
<span data-ttu-id="1859c-149">Automatisk migrering sker mellan 6:00 och 06:00:00 (lokal tid per region) under hello efter strömavbrott schema.</span><span class="sxs-lookup"><span data-stu-id="1859c-149">Automatic migrations occur between 6:00 PM and 6:00 AM (local time per region) during hello following outage schedule.</span></span>

| <span data-ttu-id="1859c-150">**Region**</span><span class="sxs-lookup"><span data-stu-id="1859c-150">**Region**</span></span> | <span data-ttu-id="1859c-151">**Uppskattat startdatum**</span><span class="sxs-lookup"><span data-stu-id="1859c-151">**Estimated start date**</span></span> | <span data-ttu-id="1859c-152">**Uppskattat slutdatum**</span><span class="sxs-lookup"><span data-stu-id="1859c-152">**Estimated end date**</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="1859c-153">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="1859c-153">Australia East</span></span> |<span data-ttu-id="1859c-154">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="1859c-154">Not determined yet</span></span> |<span data-ttu-id="1859c-155">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="1859c-155">Not determined yet</span></span> |
| <span data-ttu-id="1859c-156">Östra Kina</span><span class="sxs-lookup"><span data-stu-id="1859c-156">China East</span></span> |<span data-ttu-id="1859c-157">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-157">January 9, 2017</span></span> |<span data-ttu-id="1859c-158">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-158">January 13, 2017</span></span> |
| <span data-ttu-id="1859c-159">Norra Kina</span><span class="sxs-lookup"><span data-stu-id="1859c-159">China North</span></span> |<span data-ttu-id="1859c-160">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-160">January 9, 2017</span></span> |<span data-ttu-id="1859c-161">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-161">January 13, 2017</span></span> |
| <span data-ttu-id="1859c-162">Centrala Tyskland</span><span class="sxs-lookup"><span data-stu-id="1859c-162">Germany Central</span></span> |<span data-ttu-id="1859c-163">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-163">January 9, 2017</span></span> |<span data-ttu-id="1859c-164">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-164">January 13, 2017</span></span> |
| <span data-ttu-id="1859c-165">Nordöstra Tyskland</span><span class="sxs-lookup"><span data-stu-id="1859c-165">Germany Northeast</span></span> |<span data-ttu-id="1859c-166">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-166">January 9, 2017</span></span> |<span data-ttu-id="1859c-167">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-167">January 13, 2017</span></span> |
| <span data-ttu-id="1859c-168">Västra Indien</span><span class="sxs-lookup"><span data-stu-id="1859c-168">India West</span></span> |<span data-ttu-id="1859c-169">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="1859c-169">Not determined yet</span></span> |<span data-ttu-id="1859c-170">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="1859c-170">Not determined yet</span></span> |
| <span data-ttu-id="1859c-171">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="1859c-171">Japan West</span></span> |<span data-ttu-id="1859c-172">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="1859c-172">Not determined yet</span></span> |<span data-ttu-id="1859c-173">Inte fastställa ännu</span><span class="sxs-lookup"><span data-stu-id="1859c-173">Not determined yet</span></span> |
| <span data-ttu-id="1859c-174">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="1859c-174">North Central US</span></span> |<span data-ttu-id="1859c-175">9 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-175">January 9, 2017</span></span> |<span data-ttu-id="1859c-176">Den 13 januari 2017</span><span class="sxs-lookup"><span data-stu-id="1859c-176">January 13, 2017</span></span> |

## <a name="self-migration-toopremium-storage"></a><span data-ttu-id="1859c-177">Eget migrering toopremium lagring</span><span class="sxs-lookup"><span data-stu-id="1859c-177">Self-migration toopremium storage</span></span>
<span data-ttu-id="1859c-178">Om du vill toocontrol när din avbrott inträffar kan använda du hello följa steg toomigrate ett befintligt datalager på standardlagring toopremium lagring.</span><span class="sxs-lookup"><span data-stu-id="1859c-178">If you want toocontrol when your downtime will occur, you can use hello following steps toomigrate an existing data warehouse on standard storage toopremium storage.</span></span> <span data-ttu-id="1859c-179">Om du väljer det här alternativet måste du slutföra hello eget migrering innan automatisk migrering av hello börjar i den regionen.</span><span class="sxs-lookup"><span data-stu-id="1859c-179">If you choose this option, you must complete hello self-migration before hello automatic migration begins in that region.</span></span> <span data-ttu-id="1859c-180">Detta säkerställer att du undvika hello automatisk migrering av orsakar en konflikt (se toohello [schema för automatisk migrering av][automatic migration schedule]).</span><span class="sxs-lookup"><span data-stu-id="1859c-180">This ensures that you avoid any risk of hello automatic migration causing a conflict (refer toohello [automatic migration schedule][automatic migration schedule]).</span></span>

### <a name="self-migration-instructions"></a><span data-ttu-id="1859c-181">Egna migreringsanvisningar</span><span class="sxs-lookup"><span data-stu-id="1859c-181">Self-migration instructions</span></span>
<span data-ttu-id="1859c-182">toomigrate data warehouse dig själv, Använd hello säkerhetskopiering och återställning funktioner.</span><span class="sxs-lookup"><span data-stu-id="1859c-182">toomigrate your data warehouse yourself, use hello backup and restore features.</span></span> <span data-ttu-id="1859c-183">hello återställning del av hello migrering är förväntade tootake ungefär en timme per terabyte lagring per datalagret.</span><span class="sxs-lookup"><span data-stu-id="1859c-183">hello restore portion of hello migration is expected tootake around one hour per terabyte of storage per data warehouse.</span></span> <span data-ttu-id="1859c-184">Tookeep hello samma namn när migreringen är klar, så du hello [steg toorename under migreringen][steps toorename during migration].</span><span class="sxs-lookup"><span data-stu-id="1859c-184">If you want tookeep hello same name after migration is complete, follow hello [steps toorename during migration][steps toorename during migration].</span></span>

1. <span data-ttu-id="1859c-185">[Pausa] [ Pause] ditt data warehouse.</span><span class="sxs-lookup"><span data-stu-id="1859c-185">[Pause][Pause] your data warehouse.</span></span> <span data-ttu-id="1859c-186">Detta tar en automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="1859c-186">This takes an automatic backup.</span></span>
2. <span data-ttu-id="1859c-187">[Återställa] [ Restore] från din senaste ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="1859c-187">[Restore][Restore] from your most recent snapshot.</span></span>
3. <span data-ttu-id="1859c-188">Ta bort ditt befintliga data warehouse på standardlagring.</span><span class="sxs-lookup"><span data-stu-id="1859c-188">Delete your existing data warehouse on standard storage.</span></span> <span data-ttu-id="1859c-189">**Om du inte toodo det här steget, debiteras du för båda datalager.**</span><span class="sxs-lookup"><span data-stu-id="1859c-189">**If you fail toodo this step, you will be charged for both data warehouses.**</span></span>

> [!NOTE]
> <span data-ttu-id="1859c-190">hello utför följande inställningar inte som en del av hello migrering:</span><span class="sxs-lookup"><span data-stu-id="1859c-190">hello following settings do not carry over as part of hello migration:</span></span>
>
> * <span data-ttu-id="1859c-191">Granskning på databasnivå hello måste toobe aktiveras.</span><span class="sxs-lookup"><span data-stu-id="1859c-191">Auditing at hello database level needs toobe re-enabled.</span></span>
> * <span data-ttu-id="1859c-192">Brandväggsregler på databasnivå hello måste toobe som lagts till igen.</span><span class="sxs-lookup"><span data-stu-id="1859c-192">Firewall rules at hello database level need toobe re-added.</span></span> <span data-ttu-id="1859c-193">Brandväggsregler på servernivå hello påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="1859c-193">Firewall rules at hello server level are not affected.</span></span>
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a><span data-ttu-id="1859c-194">Byt namn på datalagret under migreringen (valfritt)</span><span class="sxs-lookup"><span data-stu-id="1859c-194">Rename data warehouse during migration (optional)</span></span>
<span data-ttu-id="1859c-195">Två databaserna på samma logiska server inte kan ha hello hello samma namn.</span><span class="sxs-lookup"><span data-stu-id="1859c-195">Two databases on hello same logical server cannot have hello same name.</span></span> <span data-ttu-id="1859c-196">SQL Data Warehouse stöder nu hello möjlighet toorename ett datalager.</span><span class="sxs-lookup"><span data-stu-id="1859c-196">SQL Data Warehouse now supports hello ability toorename a data warehouse.</span></span>

<span data-ttu-id="1859c-197">I detta exempel anta att ditt befintliga data warehouse på standardlagring med namnet ”MyDW”.</span><span class="sxs-lookup"><span data-stu-id="1859c-197">In this example, imagine that your existing data warehouse on standard storage is currently named “MyDW.”</span></span>

1. <span data-ttu-id="1859c-198">Byt namn på ”MyDW” med hjälp av hello följande ALTER DATABASE-kommandot.</span><span class="sxs-lookup"><span data-stu-id="1859c-198">Rename "MyDW" by using hello following ALTER DATABASE command.</span></span> <span data-ttu-id="1859c-199">(I det här exemplet vi ska byta namn på den ”MyDW_BeforeMigration”.)  Det här kommandot stoppar alla befintliga transaktioner och måste göras i hello huvuddatabasen toosucceed.</span><span class="sxs-lookup"><span data-stu-id="1859c-199">(In this example, we'll rename it "MyDW_BeforeMigration.")  This command stops all existing transactions, and must be done in hello master database toosucceed.</span></span>
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. <span data-ttu-id="1859c-200">[Pausa] [ Pause] ”MyDW_BeforeMigration”.</span><span class="sxs-lookup"><span data-stu-id="1859c-200">[Pause][Pause] "MyDW_BeforeMigration."</span></span> <span data-ttu-id="1859c-201">Detta tar en automatisk säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="1859c-201">This takes an automatic backup.</span></span>
3. <span data-ttu-id="1859c-202">[Återställa] [ Restore] från din senaste ögonblicksbild en ny databas med namnet hello används den toobe (till exempel ”MyDW”).</span><span class="sxs-lookup"><span data-stu-id="1859c-202">[Restore][Restore] from your most recent snapshot a new database with hello name it used toobe (for example, "MyDW").</span></span>
4. <span data-ttu-id="1859c-203">Ta bort ”MyDW_BeforeMigration”.</span><span class="sxs-lookup"><span data-stu-id="1859c-203">Delete "MyDW_BeforeMigration."</span></span> <span data-ttu-id="1859c-204">**Om du inte toodo det här steget, debiteras du för båda datalager.**</span><span class="sxs-lookup"><span data-stu-id="1859c-204">**If you fail toodo this step, you will be charged for both data warehouses.**</span></span>


## <a name="next-steps"></a><span data-ttu-id="1859c-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1859c-205">Next steps</span></span>
<span data-ttu-id="1859c-206">Ändra toopremium lagring med hello har också ett ökat antal blob-databasfilerna i hello underliggande arkitekturen för ditt informationslager.</span><span class="sxs-lookup"><span data-stu-id="1859c-206">With hello change toopremium storage, you also have an increased number of database blob files in hello underlying architecture of your data warehouse.</span></span> <span data-ttu-id="1859c-207">toomaximize hello prestandafördelarna med den här ändringen återskapa grupperade columnstore-index med hjälp av hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="1859c-207">toomaximize hello performance benefits of this change, rebuild your clustered columnstore indexes by using hello following script.</span></span> <span data-ttu-id="1859c-208">hello skriptet fungerar genom att vissa av dina befintliga data toohello ytterligare blobbar.</span><span class="sxs-lookup"><span data-stu-id="1859c-208">hello script works by forcing some of your existing data toohello additional blobs.</span></span> <span data-ttu-id="1859c-209">Om ingen åtgärd distribuera hello data naturligt över tid som du kan läsa in mer data i tabeller.</span><span class="sxs-lookup"><span data-stu-id="1859c-209">If you take no action, hello data will naturally redistribute over time as you load more data into your tables.</span></span>

<span data-ttu-id="1859c-210">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="1859c-210">**Prerequisites:**</span></span>

- <span data-ttu-id="1859c-211">hello-datalagret ska köras med 1 000 informationslagerenheter eller högre (se [skala beräkningskraft][scale compute power]).</span><span class="sxs-lookup"><span data-stu-id="1859c-211">hello data warehouse should run with 1,000 data warehouse units or higher (see [scale compute power][scale compute power]).</span></span>
- <span data-ttu-id="1859c-212">hello användaren hello skriptet ska vara i hello [mediumrc rollen] [ mediumrc role] eller högre.</span><span class="sxs-lookup"><span data-stu-id="1859c-212">hello user executing hello script should be in hello [mediumrc role][mediumrc role] or higher.</span></span> <span data-ttu-id="1859c-213">tooadd toothis användarroll, kör hello följande:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span><span class="sxs-lookup"><span data-stu-id="1859c-213">tooadd a user toothis role, execute hello following: ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````</span></span>

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table toocontrol index rebuild
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
-- Step 2: Execute index rebuilds. If script fails, hello below can be re-run toorestart where last left off.
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

<span data-ttu-id="1859c-214">Om det uppstår problem med ditt data warehouse [skapa ett supportärende] [ create a support ticket] och referera till ”migrering toopremium lagring” som hello möjlig orsak.</span><span class="sxs-lookup"><span data-stu-id="1859c-214">If you encounter any issues with your data warehouse, [create a support ticket][create a support ticket] and reference “migration toopremium storage” as hello possible cause.</span></span>

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration tooPremium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps toorename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
