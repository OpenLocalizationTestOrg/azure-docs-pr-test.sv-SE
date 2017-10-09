---
title: aaaManage historisk data i Temporala tabeller med bevarandeprincip | Microsoft Docs
description: "Lär dig hur toouse temporal kvarhållning princip tookeep historiska data under din kontroll."
services: sql-database
documentationcenter: 
author: bonova
manager: drasumic
editor: 
ms.assetid: 76cfa06a-e758-453e-942c-9f1ed6a38c2a
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sql-database
ms.date: 10/12/2016
ms.author: bonova
ms.openlocfilehash: a72a6111a6cd7322d734d08bf3852e95f5ffea8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a><span data-ttu-id="329b7-103">Hantera historisk data i Temporala tabeller med bevarandeprincip</span><span class="sxs-lookup"><span data-stu-id="329b7-103">Manage historical data in Temporal Tables with retention policy</span></span>
<span data-ttu-id="329b7-104">Temporala tabeller kan öka databasens storlek mer än vanliga tabeller, särskilt om du behåller historiska data under en längre tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="329b7-104">Temporal Tables may increase database size more than regular tables, especially if you retain historical data for a longer period of time.</span></span> <span data-ttu-id="329b7-105">Därför är bevarandeprincipen för historiska data en viktig del av planering och hantering av hello livscykeln för alla temporala tabeller.</span><span class="sxs-lookup"><span data-stu-id="329b7-105">Hence, retention policy for historical data is an important aspect of planning and managing hello lifecycle of every temporal table.</span></span> <span data-ttu-id="329b7-106">Temporala tabeller i Azure SQL Database har lätt att använda kvarhållning mekanism som hjälper dig att utföra den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="329b7-106">Temporal Tables in Azure SQL Database come with easy-to-use retention mechanism that helps you accomplish this task.</span></span>

<span data-ttu-id="329b7-107">Temporala historiktabeller kvarhållning kan konfigureras på hello tabell nivå, vilket gör att användare toocreate flexibla åldern principer.</span><span class="sxs-lookup"><span data-stu-id="329b7-107">Temporal history retention can be configured at hello individual table level, which allows users toocreate flexible aging polices.</span></span> <span data-ttu-id="329b7-108">Tillämpa temporal kvarhållning är enkel: det krävs en enda parameter toobe under tabell skapas eller schemat ändras.</span><span class="sxs-lookup"><span data-stu-id="329b7-108">Applying temporal retention is simple: it requires only one parameter toobe set during table creation or schema change.</span></span>

<span data-ttu-id="329b7-109">När du har definierat bevarandeprincip startar Azure SQL Database regelbundet kontrollera om det inte finns historiska rader som är tillgängliga för automatisk Datarensning.</span><span class="sxs-lookup"><span data-stu-id="329b7-109">After you define retention policy, Azure SQL Database starts checking regularly if there are historical rows that are eligible for automatic data cleanup.</span></span> <span data-ttu-id="329b7-110">Identifiering av matchande rader och deras tas bort från hello historiktabellen uppstå transparent, hello bakgrundsaktivitet som är schemalagda och köra hello-systemet.</span><span class="sxs-lookup"><span data-stu-id="329b7-110">Identification of matching rows and their removal from hello history table occur transparently, in hello background task that is scheduled and run by hello system.</span></span> <span data-ttu-id="329b7-111">Ålder villkor för hello historik tabellraderna kontrolleras utifrån hello kolumnen som motsvarar slutet av SYSTEM_TIME-perioden.</span><span class="sxs-lookup"><span data-stu-id="329b7-111">Age condition for hello history table rows is checked based on hello column representing end of SYSTEM_TIME period.</span></span> <span data-ttu-id="329b7-112">Om loggperioden, till exempel anges toosix månader, tabellraderna berättigad för rensning uppfyller hello följande villkor:</span><span class="sxs-lookup"><span data-stu-id="329b7-112">If retention period, for example, is set toosix months, table rows eligible for cleanup satisfy hello following condition:</span></span>

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

<span data-ttu-id="329b7-113">I föregående exempel hello, vi antas som **ValidTo** kolumnen svarar toohello slutet av SYSTEM_TIME-perioden.</span><span class="sxs-lookup"><span data-stu-id="329b7-113">In hello preceding example, we assumed that **ValidTo** column corresponds toohello end of SYSTEM_TIME period.</span></span>

## <a name="how-tooconfigure-retention-policy"></a><span data-ttu-id="329b7-114">Hur tooconfigure bevarandeprincip?</span><span class="sxs-lookup"><span data-stu-id="329b7-114">How tooconfigure retention policy?</span></span>
<span data-ttu-id="329b7-115">Innan du konfigurerar bevarandeprincip för en temporal tabell, kontrollera först om temporal historiska kvarhållning har aktiverats *på databasnivå hello*.</span><span class="sxs-lookup"><span data-stu-id="329b7-115">Before you configure retention policy for a temporal table, check first whether temporal historical retention is enabled *at hello database level*.</span></span>

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

<span data-ttu-id="329b7-116">Databasen flaggan **is_temporal_history_retention_enabled** är uppsättningen tooON som standard, men användarna kan ändra med ALTER DATABASE-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="329b7-116">Database flag **is_temporal_history_retention_enabled** is set tooON by default, but users can change it with ALTER DATABASE statement.</span></span> <span data-ttu-id="329b7-117">Det är också automatiskt set tooOFF efter [återställning vid tidpunkt](sql-database-recovery-using-backups.md) igen.</span><span class="sxs-lookup"><span data-stu-id="329b7-117">It is also automatically set tooOFF after [point in time restore](sql-database-recovery-using-backups.md) operation.</span></span> <span data-ttu-id="329b7-118">tooenable temporala historiktabeller kvarhållning rensning för databasen, kör du följande instruktion hello:</span><span class="sxs-lookup"><span data-stu-id="329b7-118">tooenable temporal history retention cleanup for your database, execute hello following statement:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> <span data-ttu-id="329b7-119">Du kan konfigurera kvarhållning för temporala tabeller, även om **is_temporal_history_retention_enabled** är AVSTÄNGD, men aktiveras automatisk rensning för föråldrade rader inte i så fall.</span><span class="sxs-lookup"><span data-stu-id="329b7-119">You can configure retention for temporal tables even if **is_temporal_history_retention_enabled** is OFF, but automatic cleanup for aged rows is not triggered in that case.</span></span>
> 
> 

<span data-ttu-id="329b7-120">Bevarandeprincip konfigureras under skapande av tabell genom att ange värdet för parametern för hello HISTORY_RETENTION_PERIOD:</span><span class="sxs-lookup"><span data-stu-id="329b7-120">Retention policy is configured during table creation by specifying value for hello HISTORY_RETENTION_PERIOD parameter:</span></span>

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

<span data-ttu-id="329b7-121">Azure SQL-databas kan du toospecify kvarhållningsperiod med olika tidsenheter: dagar, veckor, månader och år.</span><span class="sxs-lookup"><span data-stu-id="329b7-121">Azure SQL Database allows you toospecify retention period by using different time units: DAYS, WEEKS, MONTHS, and YEARS.</span></span> <span data-ttu-id="329b7-122">Om HISTORY_RETENTION_PERIOD utelämnas antas oändlig kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="329b7-122">If HISTORY_RETENTION_PERIOD is omitted, INFINITE retention is assumed.</span></span> <span data-ttu-id="329b7-123">Du kan också använda oändlig nyckelordet explicit.</span><span class="sxs-lookup"><span data-stu-id="329b7-123">You can also use INFINITE keyword explicitly.</span></span>

<span data-ttu-id="329b7-124">I vissa fall kan du kanske tooconfigure kvarhållning när tabellen skulle skapas eller toochange konfigurerat tidigare värde.</span><span class="sxs-lookup"><span data-stu-id="329b7-124">In some scenarios, you may want tooconfigure retention after table creation, or toochange previously configured value.</span></span> <span data-ttu-id="329b7-125">I så fall använder du ALTER TABLE-instruktion:</span><span class="sxs-lookup"><span data-stu-id="329b7-125">In that case use ALTER TABLE statement:</span></span>

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> <span data-ttu-id="329b7-126">Ställa in SYSTEM_VERSIONING tooOFF *bevaras inte* kvarhållning periodvärde.</span><span class="sxs-lookup"><span data-stu-id="329b7-126">Setting SYSTEM_VERSIONING tooOFF *does not preserve* retention period value.</span></span> <span data-ttu-id="329b7-127">Ställa in SYSTEM_VERSIONING tooON utan HISTORY_RETENTION_PERIOD anges explicit resultat i hello obegränsad kvarhållningsperiod.</span><span class="sxs-lookup"><span data-stu-id="329b7-127">Setting SYSTEM_VERSIONING tooON without HISTORY_RETENTION_PERIOD specified explicitly results in hello INFINITE retention period.</span></span>
> 
> 

<span data-ttu-id="329b7-128">tooreview aktuella tillstånd hello bevarandeprincip, Använd hello följande fråga kopplingar temporal kvarhållning aktivering flagg på hello databasnivå med kvarhållningsperioder för enskilda tabeller:</span><span class="sxs-lookup"><span data-stu-id="329b7-128">tooreview current state of hello retention policy, use hello following query that joins temporal retention enablement flag at hello database level with retention periods for individual tables:</span></span>

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


## <a name="how-sql-database-deletes-aged-rows"></a><span data-ttu-id="329b7-129">Hur SQL-databas tar bort föråldrade rader?</span><span class="sxs-lookup"><span data-stu-id="329b7-129">How SQL Database deletes aged rows?</span></span>
<span data-ttu-id="329b7-130">hello rensningen beror på hello indexlayout hello historiktabellen.</span><span class="sxs-lookup"><span data-stu-id="329b7-130">hello cleanup process depends on hello index layout of hello history table.</span></span> <span data-ttu-id="329b7-131">Det är viktigt toonotice som *bara historik tabeller med ett grupperat index (B-trädet eller columnstore) kan ha ändlig bevarandeprincip som konfigurerats*.</span><span class="sxs-lookup"><span data-stu-id="329b7-131">It is important toonotice that *only history tables with a clustered index (B-tree or columnstore) can have finite retention policy configured*.</span></span> <span data-ttu-id="329b7-132">En bakgrundsaktivitet skapas tooperform rensning av föråldrade data för alla temporala tabeller med begränsad Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="329b7-132">A background task is created tooperform aged data cleanup for all temporal tables with finite retention period.</span></span>
<span data-ttu-id="329b7-133">Rensa logik för hello rowstore (B-trädet) grupperat index tar bort föråldrade rad i mindre segment (upp too10K) minska trycket på databasloggen och i/o-undersystem.</span><span class="sxs-lookup"><span data-stu-id="329b7-133">Cleanup logic for hello rowstore (B-tree) clustered index deletes aged row in smaller chunks (up too10K) minimizing pressure on database log and I/O subsystem.</span></span> <span data-ttu-id="329b7-134">Även om rensning logik använder krävs B-trädindex ordning borttagningar för hello rader som är äldre än bevarandeperioden inte kan garanteras ordentligt.</span><span class="sxs-lookup"><span data-stu-id="329b7-134">Although cleanup logic utilizes required B-tree index, order of deletions for hello rows older than retention period cannot be firmly guaranteed.</span></span> <span data-ttu-id="329b7-135">Därför *gör inte alla beroende på hello Rensa ordning i dina program*.</span><span class="sxs-lookup"><span data-stu-id="329b7-135">Hence, *do not take any dependency on hello cleanup order in your applications*.</span></span>

<span data-ttu-id="329b7-136">hello uppgiften för hello grupperade columnstore tar bort hela [rad grupper](https://msdn.microsoft.com/library/gg492088.aspx) samtidigt (vanligtvis innehålla 1 miljon rader varje), vilket är mycket effektivt, särskilt när historiska data genereras i en hög takt.</span><span class="sxs-lookup"><span data-stu-id="329b7-136">hello cleanup task for hello clustered columnstore removes entire [row groups](https://msdn.microsoft.com/library/gg492088.aspx) at once (typically contain 1 million of rows each), which is very efficient, especially when historical data is generated at a high pace.</span></span>

![Det grupperade columnstore kvarhållning](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

<span data-ttu-id="329b7-138">Utmärkt datakomprimering och effektiv kvarhållning Rensa gör klustrade kolumnlagringsindexet ett perfekt val för scenarier när din arbetsbelastning genererar snabbt stora mängden historiska data.</span><span class="sxs-lookup"><span data-stu-id="329b7-138">Excellent data compression and efficient retention cleanup makes clustered columnstore index a perfect choice for scenarios when your workload rapidly generates high amount of historical data.</span></span> <span data-ttu-id="329b7-139">Mönstret är typiskt för intensiva [transaktionell bearbetning arbetsbelastningar som använder temporala tabeller](https://msdn.microsoft.com/library/mt631669.aspx) för ändringsspårning och granskning, trendanalys eller IoT datapåfyllning.</span><span class="sxs-lookup"><span data-stu-id="329b7-139">That pattern is typical for intensive [transactional processing workloads that use temporal tables](https://msdn.microsoft.com/library/mt631669.aspx) for change tracking and auditing, trend analysis, or IoT data ingestion.</span></span>

## <a name="index-considerations"></a><span data-ttu-id="329b7-140">Index-överväganden</span><span class="sxs-lookup"><span data-stu-id="329b7-140">Index considerations</span></span>
<span data-ttu-id="329b7-141">Hej rensningsuppgiften för tabeller med rowstore grupperat index kräver index toostart där hello kolumnen motsvarande hello SYSTEM_TIME-perioden.</span><span class="sxs-lookup"><span data-stu-id="329b7-141">hello cleanup task for tables with rowstore clustered index requires index toostart with hello column corresponding hello end of SYSTEM_TIME period.</span></span> <span data-ttu-id="329b7-142">Om index inte finns kan du konfigurera en begränsad period:</span><span class="sxs-lookup"><span data-stu-id="329b7-142">If such index doesn't exist, you cannot configure a finite retention period:</span></span>

<span data-ttu-id="329b7-143">*Meddelande 13765, nivå 16, tillstånd 1 <br> </br> ändligt kvarhållningsperiod gick inte att ange på temporal systemversionstabell temporalstagetestdb.dbo.WebsiteUserInfo eftersom hello historiktabellen ' temporalstagetestdb.dbo.WebsiteUserInfoHistory' innehåller inte nödvändiga grupperat index. Överväg att skapa ett grupperat columnstore- eller B-trädet index som börjar med hello-kolumn som matchar slutet av SYSTEM_TIME period för hello historiktabell.*</span><span class="sxs-lookup"><span data-stu-id="329b7-143">*Msg 13765, Level 16, State 1 <br></br> Setting finite retention period failed on system-versioned temporal table 'temporalstagetestdb.dbo.WebsiteUserInfo' because hello history table 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' does not contain required clustered index. Consider creating a clustered columnstore or B-tree index starting with hello column that matches end of SYSTEM_TIME period, on hello history table.*</span></span>

<span data-ttu-id="329b7-144">Det är viktigt toonotice som hello standard historiktabellen som skapats av Azure SQL Database redan har grupperat index som är godkända för bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="329b7-144">It is important toonotice that hello default history table created by Azure SQL Database already has clustered index, which is compliant for retention policy.</span></span> <span data-ttu-id="329b7-145">Om du försöker tooremove index för en tabell med begränsad Bevarandeperiod, misslyckas åtgärden med hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="329b7-145">If you try tooremove that index on a table with finite retention period, operation fails with hello following error:</span></span>

<span data-ttu-id="329b7-146">*Meddelande 13766, nivå 16, tillstånd 1 <br> </br> går inte att släppa hello grupperat index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' eftersom den används för automatisk rensning av föråldrade data. Överväg att inställningen HISTORY_RETENTION_PERIOD tooINFINITE på hello motsvarande temporal systemversionstabell om du behöver toodrop detta index.*</span><span class="sxs-lookup"><span data-stu-id="329b7-146">*Msg 13766, Level 16, State 1 <br></br> Cannot drop hello clustered index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' because it is being used for automatic cleanup of aged data. Consider setting HISTORY_RETENTION_PERIOD tooINFINITE on hello corresponding system-versioned temporal table if you need toodrop this index.*</span></span>

<span data-ttu-id="329b7-147">Rensa på hello grupperade columnstore-indexet fungerar optimalt om historiska rader infogas i stigande ordning (ordnade hello slutet av periodkolumnen), vilket är alltid hello fallet när hello historiktabellen fylls exklusivt av hello SYSTEM_ hello VERSIONIOING mekanism.</span><span class="sxs-lookup"><span data-stu-id="329b7-147">Cleanup on hello clustered columnstore index works optimally if historical rows are inserted in hello ascending order (ordered by hello end of period column), which is always hello case when hello history table is populated exclusively by hello SYSTEM_VERSIONIOING mechanism.</span></span> <span data-ttu-id="329b7-148">Om rader i hello historiktabellen inte sorteras efter slutet av periodkolumnen (vilket kan vara hello fallet om du har migrerat befintliga historiska data), bör du återskapa grupperade columnstore-indexet ovanpå B-trädet rowstore-index korrekt beställda tooachieve optimala prestanda.</span><span class="sxs-lookup"><span data-stu-id="329b7-148">If rows in hello history table are not ordered by end of period column (which may be hello case if you migrated existing historical data), you should re-create clustered columnstore index on top of B-tree rowstore index that is properly ordered, tooachieve optimal performance.</span></span>

<span data-ttu-id="329b7-149">Undvik återskapa grupperat columnstore-index på hello historiktabellen med hello ändligt Bevarandeperiod eftersom den kan ändra ordning i hello radgrupper naturligt införts av hello systemversionshanteringen åtgärden.</span><span class="sxs-lookup"><span data-stu-id="329b7-149">Avoid rebuilding clustered columnstore index on hello history table with hello finite retention period, because it may change ordering in hello row groups naturally imposed by hello system-versioning operation.</span></span> <span data-ttu-id="329b7-150">Om du behöver toorebuild grupperat columnstore-index på hello historiktabellen, gör du genom att återskapa ovanpå kompatibla B-trädindex, bevarar sortering i hello rowgroups krävs för att rensa vanliga data.</span><span class="sxs-lookup"><span data-stu-id="329b7-150">If you need toorebuild clustered columnstore index on hello history table, do that by re-creating it on top of compliant B-tree index, preserving ordering in hello rowgroups necessary for regular data cleanup.</span></span> <span data-ttu-id="329b7-151">hello samma metod som ska vidtas om du skapar temporal tabell med befintliga historiktabellen som innehåller grupperat kolumnindex utan garanterad dataordning:</span><span class="sxs-lookup"><span data-stu-id="329b7-151">hello same approach should be taken if you create temporal table with existing history table that has clustered column index without guaranteed data order:</span></span>

````
/*Create B-tree ordered by hello end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

<span data-ttu-id="329b7-152">När begränsat kvarhållningsperiod har konfigurerats för hello historiktabellen med hello grupperat columnstore-index kan skapa du inte ytterligare icke-grupperade B-trädindex på tabellen:</span><span class="sxs-lookup"><span data-stu-id="329b7-152">When finite retention period is configured for hello history table with hello clustered columnstore index, you cannot create additional non-clustered B-tree indexes on that table:</span></span>

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

<span data-ttu-id="329b7-153">Ett försök tooexecute ovan instruktionen misslyckas med hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="329b7-153">An attempt tooexecute above statement fails with hello following error:</span></span>

<span data-ttu-id="329b7-154">*Meddelande 13772, nivå 16, tillstånd 1 <br> </br> kan inte skapa icke-grupperat index i en temporal historiktabell 'WebsiteUserInfoHistory' eftersom den har begränsad Bevarandeperiod och grupperat columnstore-index som definierats.*</span><span class="sxs-lookup"><span data-stu-id="329b7-154">*Msg 13772, Level 16, State 1 <br></br> Cannot create non-clustered index on a temporal history table 'WebsiteUserInfoHistory' since it has finite retention period and clustered columnstore index defined.*</span></span>

## <a name="querying-tables-with-retention-policy"></a><span data-ttu-id="329b7-155">Skicka frågor till tabeller med bevarandeprincip</span><span class="sxs-lookup"><span data-stu-id="329b7-155">Querying tables with retention policy</span></span>
<span data-ttu-id="329b7-156">Alla frågor på hello temporal tabell automatiskt filtrera ut historiska rader som matchar ändlig bevarandeprincip tooavoid oförutsägbart och inkonsekventa resultat, eftersom föråldrade rader kan tas bort av hello rensningsuppgiften *när som helst i tid och i godtycklig ordning*.</span><span class="sxs-lookup"><span data-stu-id="329b7-156">All queries on hello temporal table automatically filter out historical rows matching finite retention policy, tooavoid unpredictable and inconsistent results, since aged rows can be deleted by hello cleanup task, *at any point in time and in arbitrary order*.</span></span>

<span data-ttu-id="329b7-157">hello visar följande bild hello frågeplan för en enkel fråga:</span><span class="sxs-lookup"><span data-stu-id="329b7-157">hello following picture shows hello query plan for a simple query:</span></span>

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

<span data-ttu-id="329b7-158">hello frågan planen innehåller ytterligare filter tillämpas tooend av periodkolumnen (ValidTo) i hello grupperat Index skanna operatorn på hello historiktabellen (markerat).</span><span class="sxs-lookup"><span data-stu-id="329b7-158">hello query plan includes additional filter applied tooend of period column (ValidTo) in hello Clustered Index Scan operator on hello history table (highlighted).</span></span> <span data-ttu-id="329b7-159">Det här exemplet förutsätter att en månad kvarhållningsperioden har angetts för WebsiteUserInfo tabellen.</span><span class="sxs-lookup"><span data-stu-id="329b7-159">This example assumes that one MONTH retention period was set on WebsiteUserInfo table.</span></span>

![Kvarhållning frågefilter](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

<span data-ttu-id="329b7-161">Om du frågar historiktabellen direkt kan du se rader som är äldre än angivna period, men utan några garanti för repeterbara frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="329b7-161">However, if you query history table directly, you may see rows that are older than specified retention period, but without any guarantee for repeatable query results.</span></span> <span data-ttu-id="329b7-162">hello följande bild visas körning av frågeplan för hello frågan på hello historiktabellen utan ytterligare filter:</span><span class="sxs-lookup"><span data-stu-id="329b7-162">hello following picture shows query execution plan for hello query on hello history table without additional filters applied:</span></span>

![Frågar historik utan bevarande filtret](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

<span data-ttu-id="329b7-164">Förlita dig inte affärslogik på läsa historiktabellen utöver kvarhållningsperiod som du kan få inkonsekventa eller oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="329b7-164">Do not rely your business logic on reading history table beyond retention period as you may get inconsistent or unexpected results.</span></span> <span data-ttu-id="329b7-165">Vi rekommenderar att du använder temporala frågor med FOR SYSTEM_TIME-satsen för att analysera data i temporala tabeller.</span><span class="sxs-lookup"><span data-stu-id="329b7-165">We recommend that you use temporal queries with FOR SYSTEM_TIME clause for analyzing data in temporal tables.</span></span>

## <a name="point-in-time-restore-considerations"></a><span data-ttu-id="329b7-166">Peka i tid överväganden för återställning</span><span class="sxs-lookup"><span data-stu-id="329b7-166">Point in time restore considerations</span></span>
<span data-ttu-id="329b7-167">När du skapar en ny databas genom att [återställa befintlig databas tooa specifika punkt i tiden](sql-database-recovery-using-backups.md), den har temporal kvarhållning inaktiveras när hello databasnivå.</span><span class="sxs-lookup"><span data-stu-id="329b7-167">When you create new database by [restoring existing database tooa specific point in time](sql-database-recovery-using-backups.md), it has temporal retention disabled at hello database level.</span></span> <span data-ttu-id="329b7-168">(**is_temporal_history_retention_enabled** tooOFF-flaggan).</span><span class="sxs-lookup"><span data-stu-id="329b7-168">(**is_temporal_history_retention_enabled** flag set tooOFF).</span></span> <span data-ttu-id="329b7-169">Den här funktionen tillåter tooexamine alla historiska rader vid en återställning utan att bekymra dig som föråldrade rader tas bort innan du når tooquery dem.</span><span class="sxs-lookup"><span data-stu-id="329b7-169">This functionality allows you tooexamine all historical rows upon restore, without worrying that aged rows are removed before you get tooquery them.</span></span> <span data-ttu-id="329b7-170">Du kan använda för*inspektera historiska data utöver konfigurerade bevarandeperioden*.</span><span class="sxs-lookup"><span data-stu-id="329b7-170">You can use it too*inspect historical data beyond configured retention period*.</span></span>

<span data-ttu-id="329b7-171">Anta att en temporal tabell har en månad kvarhållning tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="329b7-171">Say that a temporal table has one MONTH retention period specified.</span></span> <span data-ttu-id="329b7-172">Om din databas har skapats i Premium tjänstnivå vore kan toocreate databaskopian med hello databastillståndet in too35 dagar bakåt i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="329b7-172">If your database was created in Premium Service tier, you would be able toocreate database copy with hello database state up too35 days back in hello past.</span></span> <span data-ttu-id="329b7-173">Som ett effektivt sätt att du tooanalyze historiska rader som är igång too65 dagar gammal genom att fråga hello historiktabellen direkt.</span><span class="sxs-lookup"><span data-stu-id="329b7-173">That effectively would allow you tooanalyze historical rows that are up too65 days old by querying hello history table directly.</span></span>

<span data-ttu-id="329b7-174">Om du vill tooactivate temporal kvarhållning rensning, kör du hello följande Transact-SQL-instruktion efter tidpunkt för återställning:</span><span class="sxs-lookup"><span data-stu-id="329b7-174">If you want tooactivate temporal retention cleanup, run hello following Transact-SQL statement after point in time restore:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a><span data-ttu-id="329b7-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="329b7-175">Next steps</span></span>
<span data-ttu-id="329b7-176">hur toouse Temporala tabeller i dina program kolla toolearn [komma igång med Temporala tabeller i Azure SQL Database](sql-database-temporal-tables.md).</span><span class="sxs-lookup"><span data-stu-id="329b7-176">toolearn how toouse Temporal Tables in your applications, check out [Getting Started with Temporal Tables in Azure SQL Database](sql-database-temporal-tables.md).</span></span>

<span data-ttu-id="329b7-177">Besök Channel 9 toohear en [verkliga kunden temporal implementering fungerat](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) och titta på en [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="329b7-177">Visit Channel 9 toohear a [real customer temporal implementation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

<span data-ttu-id="329b7-178">Detaljerad information om Temporala tabeller, granska [MSDN-dokumentationen](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="329b7-178">For detailed information about Temporal Tables, review [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>

