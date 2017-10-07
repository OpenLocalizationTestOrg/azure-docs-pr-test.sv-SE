---
title: aaaSQL Server-databas migrering tooAzure SQL-databas | Microsoft Docs
description: "Lär dig hur är det med SQL Server-databas migrering tooAzure SQL-databas i hello molnet. Använd databas migrering verktyg tootest kompatibilitet tidigare toodatabase migrering."
keywords: databasmigrering, sql server-databasmigrering, databasmigreringsverktyg, migrera databas, migrera sql-databas
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 02/08/2017
ms.author: carlrab
ms.openlocfilehash: 3a5e879404dd2da1dd5254a6134e3ee1517648db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-database-migration-toosql-database-in-hello-cloud"></a>SQL Server-databas migrering tooSQL databasen i hello moln
I den här artikeln får information du om hello två huvudsakliga sätt för att migrera en SQL Server 2005 eller senare databas tooAzure SQL-databas. hello första metoden är enklare, men kräver vissa, möjligen betydande driftstopp under hello migreringen. hello andra metoden är mer komplexa, men eliminerar avsevärt avbrott under hello migreringen.

I båda fallen måste tooensure hello källa databasen är kompatibel med Azure SQL Database med hello [Data migrering Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595). SQL Database V12 närmar sig [har paritet](sql-database-features.md) med SQL Server än åtgärder för problem relaterade tooserver nivå och flera databaser. Databaser och program som förlitar sig på [delvis stöds eller som inte stöds funktioner](sql-database-transact-sql-information.md) måste vissa [omarbetningar toofix dessa inkompatibiliteter](sql-database-cloud-migrate.md#resolving-database-migration-compatibility-issues) innan hello SQL Server databasen kan migreras.

> [!NOTE]
> toomigrate en SQL Server-databas, inklusive Microsoft Access, Sybase, MySQL Oracle och DB2 tooAzure SQL-databas finns [SQL Server Migration Assistant](https://blogs.msdn.microsoft.com/datamigration/2016/12/22/released-sql-server-migration-assistant-ssma-v7-2/).
> 

## <a name="method-1-migration-with-downtime-during-hello-migration"></a>Metod 1: Migrering avbrott under migreringen hello

 Om du har råd med en viss stilleståndstid eller om du utför en testmigrering av en produktionsdatabas som ska migreras senare, kan du använda den här metoden. En självstudiekurs finns [migrera en SQL Server-databas](sql-database-migrate-your-sql-server-database.md).

hello innehåller följande lista hello allmänna arbetsflödet för SQL Server-Databasmigrering med den här metoden.

  ![VSSSDT-migreringsdiagram](./media/sql-database-cloud-migrate/azure-sql-migration-sql-db.png)

1. Utvärdera hello databas för kompatibilitet med hello senaste versionen av [Data migrering Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).
2. Förbered nödvändiga korrigeringar som Transact-SQL-skript.
3. Hello källdatabasen transaktionellt konsekvent kopiera migreras - och se till att inga ytterligare ändringar görs toohello källdatabasen (eller kan du manuellt koppla sådana ändringar när hello migreringen har slutförts). Det finns många metoder tooquiesce en databas från att inaktivera klienten anslutning toocreating en [databasögonblicksbilden](https://msdn.microsoft.com/library/ms175876.aspx).
4. Distribuera hello Transact-SQL-skript tooapply hello korrigeringar toohello databaskopian.
5. [Exportera](sql-database-export.md) hello tooa för kopia av databasen. BACPAC fil på en lokal enhet.
6. [Importera](sql-database-import.md) hello. BACPAC filen som en ny Azure SQL-databas med hjälp av flera BACPAC importera verktyg, med SQLPackage.exe som hello rekommenderas för bästa prestanda.

### <a name="optimizing-data-transfer-performance-during-migration"></a>Optimera prestanda för dataöverföring under migreringen 

hello följande lista innehåller rekommendationer för bästa prestanda under hello importen.

* Välj hello högsta servicenivå och prestanda på datanivå att din budget tillåter toomaximize hello överföring prestanda. Du kan skala när hello migreringen har slutförts toosave pengar. 
* Minimera hello avståndet mellan din. BACPAC fil- och hello mål datacenter.
* Inaktivera automatisk statistik under migreringen
* Partitionstabeller och index
* Ta bort indexerade vyer och återskapa dem när de är klara
* Ta bort sällan efterfrågade historiska data tooanother databasen och migrera historiska data tooa separat Azure SQL-databasen. Du kan sedan söka i historiska data med [elastiska frågor](sql-database-elastic-query-overview.md).

### <a name="optimize-performance-after-hello-migration-completes"></a>Optimera prestanda när hello migreringen har slutförts

[Uppdatera statistik](https://msdn.microsoft.com/library/ms187348.aspx) med fullständig genomsökning efter hello migreringen är klar.

## <a name="method-2-use-transactional-replication"></a>Metod 2: Använd transaktionsreplikering

När du inte har råd tooremove SQL Server-databasen från produktionen medan hello migreringen sker, använder du Transaktionsreplikering i SQL Server som din migreringslösning. toouse måste den här metoden hello källdatabasen uppfyller hello [krav för Transaktionsreplikering](https://msdn.microsoft.com/library/mt589530.aspx) och vara kompatibla för Azure SQL Database. Information om SQL-replikeringen med AlwaysOn finns [konfigurerar replikering för Always On-Tillgänglighetsgrupper (SQL Server)](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server).

toouse den här lösningen konfigurerar du din Azure SQL-databas som en prenumerant toohello SQL Server-instans som du vill toomigrate. hello Transaktionsreplikering distributören synkroniseras data från hello databasen toobe synkroniseras (hello utgivare) medan nya transaktioner fortsätter inträffa. 

Med Transaktionsreplikering, alla ändringar tooyour data eller schemat visas i Azure SQL-databasen. När hello synkroniseringen är klar och du är redo toomigrate, ändra hello anslutningssträngen för ditt program toopoint dem tooyour Azure SQL Database. När Transaktionsreplikering tömmer ändringar kvar peka tooAzure DB på källdatabasen och alla program kan du avinstallera Transaktionsreplikering. Nu är Azure SQL Database ditt produktionssystem.

 ![SeedCloudTR-diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

> [!TIP]
> Du kan också använda Transaktionsreplikering toomigrate en delmängd av källdatabasen. hello publikationen du replikera tooAzure SQL-databasen kan vara begränsad tooa delmängd av hello tabeller i hello databasen håller på att replikeras. Du kan begränsa hello data tooa delmängd av hello rader och/eller en delmängd av hello kolumner för varje tabell som håller på att replikeras.
>

### <a name="migration-toosql-database-using-transaction-replication-workflow"></a>Migrering tooSQL databasen med hjälp av Transaktionsreplikering arbetsflöde

> [!IMPORTANT]
> Använd hello senaste versionen av SQL Server Management Studio tooremain synkroniseras med uppdateringar tooMicrosoft Azure och SQL-databas. Äldre versioner av SQL Server Management Studio kan inte konfigurera SQL Database som en prenumerant. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 

1. Konfigurera distribution
   -  [Med SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_1)
   -  [Med Transact-SQL](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_2)
2. Skapa publikation
   -  [Med SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_1)
   -  [Med Transact-SQL](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_2)
3. Skapa en prenumeration
   -  [Med SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_0)
   -  [Med Transact-SQL](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_1)

### <a name="some-tips-and-differences-for-migrating-toosql-database"></a>Några tips och skillnader för att migrera tooSQL databas

1. Använda en lokal distributör 
   - Detta medför att påverka prestanda på hello-servern. 
   - Om hello prestandapåverkan är acceptabel kan du använda en annan server men det lägger till komplexitet i hantering och administration.
2. När du väljer en mapp för ögonblicksbilder, se till att hello mapp som du väljer är tillräckligt stor toohold en BCP för varje tabell vill du tooreplicate. 
3. Ögonblicksbild skapas Lås Hej associerade tabellerna tills den är klar, så att schemalägga din ögonblicksbild på lämpligt sätt. 
4. Endast push-prenumerationer stöds i Azure SQL Database. Du kan bara lägga till prenumeranter från hello källdatabasen.

## <a name="resolving-database-migration-compatibility-issues"></a>Åtgärda kompatibilitetsproblem vid databasmigrering
Det finns en mängd olika kompatibilitetsproblem som kan uppstå, beroende både på hello version av SQL Server i hello käll-databasen och hello komplexitet hello-databasen som du migrerar. Äldre versioner av SQL Server har fler kompatibilitetsproblem. Använda hello efter resurser måste dessutom tooa riktade Internetsökning med hjälp av din sökmotor alternativ:

* [SQL Server-databasfunktioner som inte stöds i Azure SQL Database](sql-database-transact-sql-information.md)
* [Utgångna databasmotorfunktioner i SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Utgångna databasmotorfunktioner i SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Utgångna databasmotorfunktioner i SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Utgångna databasmotorfunktioner i SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Utgångna databasmotorfunktioner i SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Dessutom toosearching hello Internet och använder dessa resurser använder hello [MSDN SQL Server community-forumen](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) eller [StackOverflow](http://stackoverflow.com/).

## <a name="next-steps"></a>Nästa steg
* Använda hello skript på hello Azure SQL EMEA ingenjörer blogginlägg för[övervaka tempdb användningen under migreringen](https://blogs.msdn.microsoft.com/azuresqlemea/2016/12/28/lesson-learned-10-monitoring-tempdb-usage/).
* Använda hello skript på hello Azure SQL EMEA ingenjörer blogginlägg för[övervaka hello transaktion loggutrymme för din databas medan migreringen sker](https://blogs.msdn.microsoft.com/azuresqlemea/2016/10/31/lesson-learned-7-monitoring-the-transaction-log-space-of-my-database/0).
* Bloggen om att migrera med BACPAC filer finns i en SQL Server Customer Advisory Team [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Information om hur du arbetar med UTC-tid efter migreringen finns [ändra hello standardtidzon för den lokala tidszonen](https://blogs.msdn.microsoft.com/azuresqlemea/2016/07/27/lesson-learned-4-modifying-the-default-time-zone-for-your-local-time-zone/).
* Information om hur du ändrar hello standardspråk för en databas efter migreringen finns [hur toochange hello standardspråk för Azure SQL Database](https://blogs.msdn.microsoft.com/azuresqlemea/2017/01/13/lesson-learned-16-how-to-change-the-default-language-of-azure-sql-database/).


