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
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Hantera historisk data i Temporala tabeller med bevarandeprincip
Temporala tabeller kan öka databasens storlek mer än vanliga tabeller, särskilt om du behåller historiska data under en längre tidsperiod. Därför är bevarandeprincipen för historiska data en viktig del av planering och hantering av hello livscykeln för alla temporala tabeller. Temporala tabeller i Azure SQL Database har lätt att använda kvarhållning mekanism som hjälper dig att utföra den här uppgiften.

Temporala historiktabeller kvarhållning kan konfigureras på hello tabell nivå, vilket gör att användare toocreate flexibla åldern principer. Tillämpa temporal kvarhållning är enkel: det krävs en enda parameter toobe under tabell skapas eller schemat ändras.

När du har definierat bevarandeprincip startar Azure SQL Database regelbundet kontrollera om det inte finns historiska rader som är tillgängliga för automatisk Datarensning. Identifiering av matchande rader och deras tas bort från hello historiktabellen uppstå transparent, hello bakgrundsaktivitet som är schemalagda och köra hello-systemet. Ålder villkor för hello historik tabellraderna kontrolleras utifrån hello kolumnen som motsvarar slutet av SYSTEM_TIME-perioden. Om loggperioden, till exempel anges toosix månader, tabellraderna berättigad för rensning uppfyller hello följande villkor:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

I föregående exempel hello, vi antas som **ValidTo** kolumnen svarar toohello slutet av SYSTEM_TIME-perioden.

## <a name="how-tooconfigure-retention-policy"></a>Hur tooconfigure bevarandeprincip?
Innan du konfigurerar bevarandeprincip för en temporal tabell, kontrollera först om temporal historiska kvarhållning har aktiverats *på databasnivå hello*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Databasen flaggan **is_temporal_history_retention_enabled** är uppsättningen tooON som standard, men användarna kan ändra med ALTER DATABASE-instruktionen. Det är också automatiskt set tooOFF efter [återställning vid tidpunkt](sql-database-recovery-using-backups.md) igen. tooenable temporala historiktabeller kvarhållning rensning för databasen, kör du följande instruktion hello:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> Du kan konfigurera kvarhållning för temporala tabeller, även om **is_temporal_history_retention_enabled** är AVSTÄNGD, men aktiveras automatisk rensning för föråldrade rader inte i så fall.
> 
> 

Bevarandeprincip konfigureras under skapande av tabell genom att ange värdet för parametern för hello HISTORY_RETENTION_PERIOD:

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

Azure SQL-databas kan du toospecify kvarhållningsperiod med olika tidsenheter: dagar, veckor, månader och år. Om HISTORY_RETENTION_PERIOD utelämnas antas oändlig kvarhållning. Du kan också använda oändlig nyckelordet explicit.

I vissa fall kan du kanske tooconfigure kvarhållning när tabellen skulle skapas eller toochange konfigurerat tidigare värde. I så fall använder du ALTER TABLE-instruktion:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> Ställa in SYSTEM_VERSIONING tooOFF *bevaras inte* kvarhållning periodvärde. Ställa in SYSTEM_VERSIONING tooON utan HISTORY_RETENTION_PERIOD anges explicit resultat i hello obegränsad kvarhållningsperiod.
> 
> 

tooreview aktuella tillstånd hello bevarandeprincip, Använd hello följande fråga kopplingar temporal kvarhållning aktivering flagg på hello databasnivå med kvarhållningsperioder för enskilda tabeller:

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


## <a name="how-sql-database-deletes-aged-rows"></a>Hur SQL-databas tar bort föråldrade rader?
hello rensningen beror på hello indexlayout hello historiktabellen. Det är viktigt toonotice som *bara historik tabeller med ett grupperat index (B-trädet eller columnstore) kan ha ändlig bevarandeprincip som konfigurerats*. En bakgrundsaktivitet skapas tooperform rensning av föråldrade data för alla temporala tabeller med begränsad Bevarandeperiod.
Rensa logik för hello rowstore (B-trädet) grupperat index tar bort föråldrade rad i mindre segment (upp too10K) minska trycket på databasloggen och i/o-undersystem. Även om rensning logik använder krävs B-trädindex ordning borttagningar för hello rader som är äldre än bevarandeperioden inte kan garanteras ordentligt. Därför *gör inte alla beroende på hello Rensa ordning i dina program*.

hello uppgiften för hello grupperade columnstore tar bort hela [rad grupper](https://msdn.microsoft.com/library/gg492088.aspx) samtidigt (vanligtvis innehålla 1 miljon rader varje), vilket är mycket effektivt, särskilt när historiska data genereras i en hög takt.

![Det grupperade columnstore kvarhållning](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

Utmärkt datakomprimering och effektiv kvarhållning Rensa gör klustrade kolumnlagringsindexet ett perfekt val för scenarier när din arbetsbelastning genererar snabbt stora mängden historiska data. Mönstret är typiskt för intensiva [transaktionell bearbetning arbetsbelastningar som använder temporala tabeller](https://msdn.microsoft.com/library/mt631669.aspx) för ändringsspårning och granskning, trendanalys eller IoT datapåfyllning.

## <a name="index-considerations"></a>Index-överväganden
Hej rensningsuppgiften för tabeller med rowstore grupperat index kräver index toostart där hello kolumnen motsvarande hello SYSTEM_TIME-perioden. Om index inte finns kan du konfigurera en begränsad period:

*Meddelande 13765, nivå 16, tillstånd 1 <br> </br> ändligt kvarhållningsperiod gick inte att ange på temporal systemversionstabell temporalstagetestdb.dbo.WebsiteUserInfo eftersom hello historiktabellen ' temporalstagetestdb.dbo.WebsiteUserInfoHistory' innehåller inte nödvändiga grupperat index. Överväg att skapa ett grupperat columnstore- eller B-trädet index som börjar med hello-kolumn som matchar slutet av SYSTEM_TIME period för hello historiktabell.*

Det är viktigt toonotice som hello standard historiktabellen som skapats av Azure SQL Database redan har grupperat index som är godkända för bevarandeprincip. Om du försöker tooremove index för en tabell med begränsad Bevarandeperiod, misslyckas åtgärden med hello följande fel:

*Meddelande 13766, nivå 16, tillstånd 1 <br> </br> går inte att släppa hello grupperat index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' eftersom den används för automatisk rensning av föråldrade data. Överväg att inställningen HISTORY_RETENTION_PERIOD tooINFINITE på hello motsvarande temporal systemversionstabell om du behöver toodrop detta index.*

Rensa på hello grupperade columnstore-indexet fungerar optimalt om historiska rader infogas i stigande ordning (ordnade hello slutet av periodkolumnen), vilket är alltid hello fallet när hello historiktabellen fylls exklusivt av hello SYSTEM_ hello VERSIONIOING mekanism. Om rader i hello historiktabellen inte sorteras efter slutet av periodkolumnen (vilket kan vara hello fallet om du har migrerat befintliga historiska data), bör du återskapa grupperade columnstore-indexet ovanpå B-trädet rowstore-index korrekt beställda tooachieve optimala prestanda.

Undvik återskapa grupperat columnstore-index på hello historiktabellen med hello ändligt Bevarandeperiod eftersom den kan ändra ordning i hello radgrupper naturligt införts av hello systemversionshanteringen åtgärden. Om du behöver toorebuild grupperat columnstore-index på hello historiktabellen, gör du genom att återskapa ovanpå kompatibla B-trädindex, bevarar sortering i hello rowgroups krävs för att rensa vanliga data. hello samma metod som ska vidtas om du skapar temporal tabell med befintliga historiktabellen som innehåller grupperat kolumnindex utan garanterad dataordning:

````
/*Create B-tree ordered by hello end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

När begränsat kvarhållningsperiod har konfigurerats för hello historiktabellen med hello grupperat columnstore-index kan skapa du inte ytterligare icke-grupperade B-trädindex på tabellen:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Ett försök tooexecute ovan instruktionen misslyckas med hello följande fel:

*Meddelande 13772, nivå 16, tillstånd 1 <br> </br> kan inte skapa icke-grupperat index i en temporal historiktabell 'WebsiteUserInfoHistory' eftersom den har begränsad Bevarandeperiod och grupperat columnstore-index som definierats.*

## <a name="querying-tables-with-retention-policy"></a>Skicka frågor till tabeller med bevarandeprincip
Alla frågor på hello temporal tabell automatiskt filtrera ut historiska rader som matchar ändlig bevarandeprincip tooavoid oförutsägbart och inkonsekventa resultat, eftersom föråldrade rader kan tas bort av hello rensningsuppgiften *när som helst i tid och i godtycklig ordning*.

hello visar följande bild hello frågeplan för en enkel fråga:

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

hello frågan planen innehåller ytterligare filter tillämpas tooend av periodkolumnen (ValidTo) i hello grupperat Index skanna operatorn på hello historiktabellen (markerat). Det här exemplet förutsätter att en månad kvarhållningsperioden har angetts för WebsiteUserInfo tabellen.

![Kvarhållning frågefilter](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Om du frågar historiktabellen direkt kan du se rader som är äldre än angivna period, men utan några garanti för repeterbara frågeresultat. hello följande bild visas körning av frågeplan för hello frågan på hello historiktabellen utan ytterligare filter:

![Frågar historik utan bevarande filtret](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Förlita dig inte affärslogik på läsa historiktabellen utöver kvarhållningsperiod som du kan få inkonsekventa eller oväntade resultat. Vi rekommenderar att du använder temporala frågor med FOR SYSTEM_TIME-satsen för att analysera data i temporala tabeller.

## <a name="point-in-time-restore-considerations"></a>Peka i tid överväganden för återställning
När du skapar en ny databas genom att [återställa befintlig databas tooa specifika punkt i tiden](sql-database-recovery-using-backups.md), den har temporal kvarhållning inaktiveras när hello databasnivå. (**is_temporal_history_retention_enabled** tooOFF-flaggan). Den här funktionen tillåter tooexamine alla historiska rader vid en återställning utan att bekymra dig som föråldrade rader tas bort innan du når tooquery dem. Du kan använda för*inspektera historiska data utöver konfigurerade bevarandeperioden*.

Anta att en temporal tabell har en månad kvarhållning tidsperioden. Om din databas har skapats i Premium tjänstnivå vore kan toocreate databaskopian med hello databastillståndet in too35 dagar bakåt i hello tidigare. Som ett effektivt sätt att du tooanalyze historiska rader som är igång too65 dagar gammal genom att fråga hello historiktabellen direkt.

Om du vill tooactivate temporal kvarhållning rensning, kör du hello följande Transact-SQL-instruktion efter tidpunkt för återställning:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a>Nästa steg
hur toouse Temporala tabeller i dina program kolla toolearn [komma igång med Temporala tabeller i Azure SQL Database](sql-database-temporal-tables.md).

Besök Channel 9 toohear en [verkliga kunden temporal implementering fungerat](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) och titta på en [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Detaljerad information om Temporala tabeller, granska [MSDN-dokumentationen](https://msdn.microsoft.com/library/dn935015.aspx).

