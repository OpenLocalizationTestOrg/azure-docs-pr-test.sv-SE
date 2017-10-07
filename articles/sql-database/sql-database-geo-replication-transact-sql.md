---
title: "aaaConfigure geo-replikering för Azure SQL-databas med Transact-SQL | Microsoft Docs"
description: "Konfigurera geo-replikering för Azure SQL Database med Transact-SQL"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d94d89a6-3234-46c5-8279-5eb8daad10ac
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/14/2017
ms.author: carlrab
ms.openlocfilehash: 295b6b12f257dfb15131d5ee28fbe65c6476352d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurera aktiv geo-replikering för Azure SQL Database med Transact-SQL

Den här artikeln beskrivs hur du tooconfigure aktiv geo-replikering för en Azure SQL-databas med Transact-SQL.

tooinitiate redundans med Transact-SQL finns [initiera en planerad eller oplanerad växling vid fel för Azure SQL-databas med Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

> [!NOTE]
> När du använder aktiv geo-replikering (läsbara sekundärservrar) för katastrofåterställning bör du konfigurera en redundansväxlingsgrupp för alla databaser i ett program tooenable automatiskt och transparent redundans. Den här funktionen är i förhandsgranskningen. Mer information finns i [automatisk redundans grupper och geo-replikering](sql-database-geo-replication-overview.md).
> 
> 

tooconfigure aktiv geo-replikering med hjälp av Transact-SQL, behöver du hello följande:

* En Azure-prenumeration
* En logisk Azure SQL Database-server <MyLocalServer> och en SQL-databas <MyDB> -hello primära databasen som du vill tooreplicate
* En eller flera logiska Azure SQL Database-servrar < MySecondaryServer(n) > - hello logiska servrar som ska vara hello servrar där du skapar sekundära databaser
* En inloggning som DBManager på hello primära
* Har db_ownership av hello lokal databas som du kommer att geo-replikering
* Vara DBManager på hello partner servrar toowhich konfigurerar du geo-replikering
* hello senaste versionen av SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Det rekommenderas att du alltid använder hello senaste versionen av Management Studio tooremain synkroniseras med uppdateringar tooMicrosoft Azure och SQL-databas. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="add-secondary-database"></a>Lägg till sekundära databas
Du kan använda hello **ALTER DATABASE** instruktionen toocreate en georeplikerad sekundär databas på en partnerserver. Du kan köra instruktionen på hello huvuddatabasen för hello server som innehåller hello databasen toobe replikeras. hello geo-replikerade databasen (hello ”primär databas”) har samma namn som hello databasen håller på att replikeras och kommer som standard har hello hello samma tjänst nivå som hello primära databasen. hello sekundära databasen kan vara läsbart eller inte kan läsas och kan vara en enskild databas eller i en elastisk pool. Mer information finns i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) och [tjänstnivåer](sql-database-service-tiers.md).
När hello sekundära databasen skapas och dirigeras börjar data replikeras asynkront från hello primära databasen. hello stegen nedan beskriver hur tooconfigure geo-replikering med Management Studio. Steg toocreate-inte kan läsas och läsbara sekundärservrar, tillhandahålls som en enskild databas eller i en elastisk pool.

> [!NOTE]
> Om det finns en databas på hello angivna partnerserver med samma namn som misslyckas hello primära databasen hello kommandot hello.
> 

### <a name="add-readable-secondary-single-database"></a>Lägga till läsbar sekundärserver (en databas)
Använd hello följande steg toocreate en läsbar sekundärserver som en enskild databas.

1. Anslut logisk tooyour Azure SQL Database-server i Management Studio.
2. Öppna databasmappen hello, expandera hello **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använder följande hello **ALTER DATABASE** instruktionen toomake en lokal databas till en primär geo-replikering med en läsbar sekundär databas på en sekundär server.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);
4. Klicka på **kör** toorun hello frågan.

### <a name="add-readable-secondary-elastic-pool"></a>Lägga till läsbar sekundärserver (elastisk pool)
Använd följande steg toocreate en läsbar sekundärserver i en elastisk pool hello.

1. Anslut logisk tooyour Azure SQL Database-server i Management Studio.
2. Öppna databasmappen hello, expandera hello **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använder följande hello **ALTER DATABASE** instruktionen toomake en lokal databas till en primär geo-replikering med en läsbar sekundär databas på en sekundär server i en elastisk pool.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));
4. Klicka på **kör** toorun hello frågan.

## <a name="remove-secondary-database"></a>Ta bort sekundära databas
Du kan använda hello **ALTER DATABASE** instruktionen toopermanently avsluta hello replikering partnerskap mellan en sekundär databas och dess primära. Den här instruktionen körs på hello master-databasen på vilka hello primära databasen finns. När hello relationen avslutades blir hello sekundär databas en vanlig skrivskyddad databas. Om hello anslutningen toosecondary databasen är skadad hello kommando lyckas men hello sekundära blir oskyddad när anslutningen har återställts. Mer information finns i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) och [tjänstnivåer](sql-database-service-tiers.md).

Använd följande steg tooremove georeplikerad sekundära från ett geo-replikering partnerskap hello.

1. Anslut logisk tooyour Azure SQL Database-server i Management Studio.
2. Öppna databasmappen hello, expandera hello **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använder följande hello **ALTER DATABASE** instruktionen tooremove en georeplikerad sekundär.
   
        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;
4. Klicka på **kör** toorun hello frågan.

## <a name="monitor-active-geo-replication-configuration-and-health"></a>Övervakarkonfiguration aktiv geo-replikering och hälsa

Övervakningsaktiviteter är övervakning av hello geo-replikering konfiguration och övervakning av hälsotillstånd för replikering av data.  Du kan använda hello **sys.dm_geo_replication_links** dynamisk hanteringsvy i hello huvuddatabasen tooreturn information om alla befintliga replikeringslänkar för varje databas på logisk hello Azure SQL Database-server. Den här vyn innehåller en rad för varje hello replikeringslänken mellan primära och sekundära databaser. Du kan använda hello **sys.dm_replication_link_status** hantering av dynamiska visa tooreturn en rad för varje Azure SQL Database som för närvarande används i en replikeringslänk för replikering. Detta inkluderar både primära och sekundära databaser. Om mer än en kontinuerlig replikeringslänk finns för en viss primär databas i den här tabellen innehåller en rad för varje hello relationer. hello vyn har skapats i alla databaser, inklusive hello logiska master. Dock returnerar frågor till den här vyn i hello logiska master en tom uppsättning. Du kan använda hello **sys.dm_operation_status** hantering av dynamiska visa tooshow hello status för alla åtgärder, inklusive hello statusen för replikeringslänkar för hello-databas. Mer information finns i [sys.geo_replication_links (Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx), och [sys.dm_operation_status (Azure SQL Databas)](https://msdn.microsoft.com/library/dn270022.aspx).

Använd följande steg toomonitor en aktiv geo-replikering partnerskap hello.

1. Anslut logisk tooyour Azure SQL Database-server i Management Studio.
2. Öppna databasmappen hello, expandera hello **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använd följande instruktion tooshow hello alla databaser med geo-replikeringslänkar.
   
        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM sys.geo_replication_links;
4. Klicka på **kör** toorun hello frågan.
5. Öppna databasmappen hello, expandera hello **systemdatabaser** mappen, högerklicka på **MyDB**, och klicka sedan på **ny fråga**.
6. Använd hello följande instruktion tooshow hello replikering ligger och senast replikering av min sekundära MyDB-databaser.
   
        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status
7. Klicka på **kör** toorun hello frågan.
8. Använd följande instruktion tooshow hello senaste geo-replikering åtgärder som associeras med databasen MyDB hello.
   
        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC
9. Klicka på **kör** toorun hello frågan.

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om växling vid fel grupper och aktiv geo-replikering, se - [redundans grupper](sql-database-geo-replication-overview.md)
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md)

