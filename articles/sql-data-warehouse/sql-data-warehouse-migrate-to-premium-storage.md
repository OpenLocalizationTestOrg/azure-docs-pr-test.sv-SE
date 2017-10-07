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
# <a name="migrate-your-data-warehouse-toopremium-storage"></a>Migrera dina data warehouse toopremium lagring
Azure SQL Data Warehouse som nyligen lades [premium-lagring för bättre prestanda förutsägbarhet][premium storage for greater performance predictability]. Befintliga data distributionslager för närvarande på standardlagring ska migreras toopremium lagring. Du kan dra nytta av automatisk migrering eller om du föredrar toocontrol när toomigrate (som inbegriper vissa avbrott), kan du göra hello migrering själv.

Om du har mer än ett datalager, använder hello [schema för automatisk migrering av] [ automatic migration schedule] toodetermine när den flyttas.

## <a name="determine-storage-type"></a>Fastställa lagringstyp
Om du har skapat ett informationslager innan hello efter datum använder du standardlagring.

| **Region** | **Datalagret som skapats före detta datum** |
|:--- |:--- |
| Östra Australien |Premium-lagring är inte tillgänglig än |
| Östra Kina |Den 1 november 2016 |
| Norra Kina |Den 1 november 2016 |
| Centrala Tyskland |Den 1 november 2016 |
| Nordöstra Tyskland |Den 1 november 2016 |
| Västra Indien |Premium-lagring är inte tillgänglig än |
| Västra Japan |Premium-lagring är inte tillgänglig än |
| Norra centrala USA |10 november 2016 |

## <a name="automatic-migration-details"></a>Automatisk migrering av information
Som standard kommer vi att migrera din databas du mellan 6:00 och 06:00:00 i din region lokal tid under hello [schema för automatisk migrering av][automatic migration schedule]. Ditt befintliga data warehouse är oanvändbar under hello migreringen. hello migreringen tar ungefär en timme per terabyte lagring per datalagret. Du debiteras inte under delar av hello automatisk migrering.

> [!NOTE]
> När hello migreringen är klar att ditt data warehouse tillbaka online och kan användas.
>
>

Microsoft tar hello följande steg toocomplete hello migrering (dessa inte kräver någon inblandning från din sida). I detta exempel anta att ditt befintliga data warehouse på standardlagring med namnet ”MyDW”.

1. Microsoft byter namn på ”MyDW” för ”MyDW_DO_NOT_USE_ [tidsstämpel]”.
2. Microsoft pausar ”MyDW_DO_NOT_USE_ [tidsstämpel]”. Under den här tiden kan görs en säkerhetskopia. Du kan se flera pausar och återupptar om det uppstår problem under den här processen.
3. Microsoft skapar ett nytt datalager med namnet ”MyDW” på premium-lagring från hello säkerhetskopia i steg 2. ”MyDW” visas inte förrän efter hello återställningen är slutförd.
4. När hello återställningen är klar ”MyDW” returnerar toohello samma data warehouse enheter och -läget (pausas eller aktiv) som den var innan hello migreringen.
5. När hello migreringen är klar, Microsoft tar bort ”MyDW_DO_NOT_USE_ [tidsstämpel]”.

> [!NOTE]
> hello utför följande inställningar inte som en del av hello migrering:
>
> * Granskning på databasnivå hello måste toobe aktiveras.
> * Brandväggsregler på databasnivå hello måste toobe som lagts till igen. Brandväggsregler på servernivå hello påverkas inte.
>
>

### <a name="automatic-migration-schedule"></a>Automatisk migrering av schemat
Automatisk migrering sker mellan 6:00 och 06:00:00 (lokal tid per region) under hello efter strömavbrott schema.

| **Region** | **Uppskattat startdatum** | **Uppskattat slutdatum** |
|:--- |:--- |:--- |
| Östra Australien |Inte fastställa ännu |Inte fastställa ännu |
| Östra Kina |9 januari 2017 |Den 13 januari 2017 |
| Norra Kina |9 januari 2017 |Den 13 januari 2017 |
| Centrala Tyskland |9 januari 2017 |Den 13 januari 2017 |
| Nordöstra Tyskland |9 januari 2017 |Den 13 januari 2017 |
| Västra Indien |Inte fastställa ännu |Inte fastställa ännu |
| Västra Japan |Inte fastställa ännu |Inte fastställa ännu |
| Norra centrala USA |9 januari 2017 |Den 13 januari 2017 |

## <a name="self-migration-toopremium-storage"></a>Eget migrering toopremium lagring
Om du vill toocontrol när din avbrott inträffar kan använda du hello följa steg toomigrate ett befintligt datalager på standardlagring toopremium lagring. Om du väljer det här alternativet måste du slutföra hello eget migrering innan automatisk migrering av hello börjar i den regionen. Detta säkerställer att du undvika hello automatisk migrering av orsakar en konflikt (se toohello [schema för automatisk migrering av][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Egna migreringsanvisningar
toomigrate data warehouse dig själv, Använd hello säkerhetskopiering och återställning funktioner. hello återställning del av hello migrering är förväntade tootake ungefär en timme per terabyte lagring per datalagret. Tookeep hello samma namn när migreringen är klar, så du hello [steg toorename under migreringen][steps toorename during migration].

1. [Pausa] [ Pause] ditt data warehouse. Detta tar en automatisk säkerhetskopiering.
2. [Återställa] [ Restore] från din senaste ögonblicksbild.
3. Ta bort ditt befintliga data warehouse på standardlagring. **Om du inte toodo det här steget, debiteras du för båda datalager.**

> [!NOTE]
> hello utför följande inställningar inte som en del av hello migrering:
>
> * Granskning på databasnivå hello måste toobe aktiveras.
> * Brandväggsregler på databasnivå hello måste toobe som lagts till igen. Brandväggsregler på servernivå hello påverkas inte.
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a>Byt namn på datalagret under migreringen (valfritt)
Två databaserna på samma logiska server inte kan ha hello hello samma namn. SQL Data Warehouse stöder nu hello möjlighet toorename ett datalager.

I detta exempel anta att ditt befintliga data warehouse på standardlagring med namnet ”MyDW”.

1. Byt namn på ”MyDW” med hjälp av hello följande ALTER DATABASE-kommandot. (I det här exemplet vi ska byta namn på den ”MyDW_BeforeMigration”.)  Det här kommandot stoppar alla befintliga transaktioner och måste göras i hello huvuddatabasen toosucceed.
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. [Pausa] [ Pause] ”MyDW_BeforeMigration”. Detta tar en automatisk säkerhetskopiering.
3. [Återställa] [ Restore] från din senaste ögonblicksbild en ny databas med namnet hello används den toobe (till exempel ”MyDW”).
4. Ta bort ”MyDW_BeforeMigration”. **Om du inte toodo det här steget, debiteras du för båda datalager.**


## <a name="next-steps"></a>Nästa steg
Ändra toopremium lagring med hello har också ett ökat antal blob-databasfilerna i hello underliggande arkitekturen för ditt informationslager. toomaximize hello prestandafördelarna med den här ändringen återskapa grupperade columnstore-index med hjälp av hello följande skript. hello skriptet fungerar genom att vissa av dina befintliga data toohello ytterligare blobbar. Om ingen åtgärd distribuera hello data naturligt över tid som du kan läsa in mer data i tabeller.

**Krav:**

- hello-datalagret ska köras med 1 000 informationslagerenheter eller högre (se [skala beräkningskraft][scale compute power]).
- hello användaren hello skriptet ska vara i hello [mediumrc rollen] [ mediumrc role] eller högre. tooadd toothis användarroll, kör hello följande:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

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

Om det uppstår problem med ditt data warehouse [skapa ett supportärende] [ create a support ticket] och referera till ”migrering toopremium lagring” som hello möjlig orsak.

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
