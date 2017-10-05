---
title: "Konfigurera geo-replikering för Azure SQL Database med Transact-SQL | Microsoft Docs"
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
ms.openlocfilehash: e5011c1c67e051616d53621b72e46ba894ca3c02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurera aktiv geo-replikering för Azure SQL Database med Transact-SQL

Den här artikeln visar hur du konfigurerar aktiv geo-replikering för en Azure SQL-databas med Transact-SQL.

Om du vill initiera redundans med Transact-SQL finns [initiera en planerad eller oplanerad växling vid fel för Azure SQL-databas med Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

> [!NOTE]
> När du använder aktiv geo-replikering (läsbara sekundärservrar) för katastrofåterställning bör du konfigurera en redundansväxlingsgrupp för alla databaser i ett program för att aktivera automatiskt och transparent redundans. Den här funktionen är i förhandsgranskningen. Mer information finns i [automatisk redundans grupper och geo-replikering](sql-database-geo-replication-overview.md).
> 
> 

Så här konfigurerar du aktiv geo-replikering med hjälp av Transact-SQL, behöver du följande:

* En Azure-prenumeration
* En logisk Azure SQL Database-server <MyLocalServer> och en SQL-databas <MyDB> -den primära databasen som du vill replikera
* En eller flera logiska Azure SQL Database-servrar < MySecondaryServer(n) > - logiska servrar som ska vara partnerservrar där du skapar sekundära databaser
* En inloggning som är DBManager på primärt
* Har db_ownership för den lokala databasen som du kommer att geo-replikering
* Vara DBManager på partner-servrar som du vill konfigurera geo-replikering
* Den senaste versionen av SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Det rekommenderas att du alltid använder den senaste versionen av Management Studio för att förbli synkroniserad med uppdateringar av Microsoft Azure och SQL Database. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="add-secondary-database"></a>Lägg till sekundära databas
Du kan använda den **ALTER DATABASE** instruktionen för att skapa en sekundär geo-replikerade databas på en partner. Du kan köra den här instruktionen i master-databasen på den server som innehåller databasen som ska replikeras. Georeplikerad (”primära databasen”) har samma namn som databasen håller på att replikeras och kommer som standard har samma tjänst-nivå som den primära databasen. Den sekundära databasen kan vara läsbart eller inte kan läsas och kan vara en enskild databas eller i en elastisk pool. Mer information finns i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) och [tjänstnivåer](sql-database-service-tiers.md).
När den sekundära databasen skapas och dirigeras börjar data replikeras asynkront från den primära databasen. Stegen nedan beskriver hur du konfigurerar geo-replikering med Management Studio. Steg för att skapa icke-läsbar och läsbara sekundärservrar som en enskild databas eller i en elastisk pool tillhandahålls.

> [!NOTE]
> Om det finns en databas på den angivna partnerservern med samma namn som den primära databasen kommer kommandot misslyckas.
> 

### <a name="add-readable-secondary-single-database"></a>Lägga till läsbar sekundärserver (en databas)
Använd följande steg för att skapa en läsbar sekundärserver som en enskild databas.

1. Anslut till din logiska Azure SQL Database-server i Management Studio.
2. Öppna mappen databaser, expandera den **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använd följande **ALTER DATABASE** instruktionen för att göra en lokal databas geo-replikering primära med en läsbar sekundär databas på en sekundär server.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);
4. Klicka på **Kör** att köra frågan.

### <a name="add-readable-secondary-elastic-pool"></a>Lägga till läsbar sekundärserver (elastisk pool)
Använd följande steg för att skapa en läsbar sekundärserver i en elastisk pool.

1. Anslut till din logiska Azure SQL Database-server i Management Studio.
2. Öppna mappen databaser, expandera den **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använd följande **ALTER DATABASE** instruktionen för att göra en lokal databas geo-replikering primära med en läsbar sekundär databas på en sekundär server i en elastisk pool.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));
4. Klicka på **Kör** att köra frågan.

## <a name="remove-secondary-database"></a>Ta bort sekundära databas
Du kan använda den **ALTER DATABASE** permanent avsluta replikering samarbetet mellan en sekundär databas och dess primära-uttryck. Den här instruktionen körs i master-databasen som den primära databasen finns. När relationen avslutning blir den sekundära databasen en vanlig skrivskyddad databas. Om anslutningen till sekundära databasen är skadad kommandot lyckas, men sekundärt blir oskyddad när anslutningen har återställts. Mer information finns i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) och [tjänstnivåer](sql-database-service-tiers.md).

Använd följande steg för att ta bort georeplikerad sekundära från ett partnerskap geo-replikering.

1. Anslut till din logiska Azure SQL Database-server i Management Studio.
2. Öppna mappen databaser, expandera den **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använd följande **ALTER DATABASE** instruktionen för att ta bort en sekundär geo-replikerade.
   
        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;
4. Klicka på **Kör** att köra frågan.

## <a name="monitor-active-geo-replication-configuration-and-health"></a>Övervakarkonfiguration aktiv geo-replikering och hälsa

Övervakningsaktiviteter är övervakning av geo-replikering konfiguration och övervakning av hälsotillstånd för replikering av data.  Du kan använda den **sys.dm_geo_replication_links** dynamisk hanteringsvy i huvuddatabasen för att returnera information om alla befintliga replikeringslänkar för varje databas på den logiska Azure SQL Database-servern. Den här vyn innehåller en rad för varje replikeringslänk mellan primära och sekundära databaser. Du kan använda den **sys.dm_replication_link_status** dynamisk hanteringsvy att returnera en rad för varje Azure SQL Database som för närvarande används i en replikeringslänk för replikering. Detta inkluderar både primära och sekundära databaser. Om mer än en kontinuerlig replikeringslänk finns för en viss primär databas i den här tabellen innehåller en rad för varje relationerna. Vyn har skapats i alla databaser, inklusive logiska master. Dock returnerar frågor till den här vyn i den logiska master en tom uppsättning. Du kan använda den **sys.dm_operation_status** dynamisk hanteringsvy och visa status för alla databas-åtgärder, inklusive statusen för länkar för databasreplikering. Mer information finns i [sys.geo_replication_links (Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx), och [sys.dm_operation_status (Azure SQL Databas)](https://msdn.microsoft.com/library/dn270022.aspx).

Använd följande steg för att övervaka ett partnerskap för aktiv geo-replikering.

1. Anslut till din logiska Azure SQL Database-server i Management Studio.
2. Öppna mappen databaser, expandera den **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använd följande instruktion om du vill visa alla databaser med geo-replikeringslänkar.
   
        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM sys.geo_replication_links;
4. Klicka på **Kör** att köra frågan.
5. Öppna mappen databaser, expandera den **systemdatabaser** mappen, högerklicka på **MyDB**, och klicka sedan på **ny fråga**.
6. Använder följande instruktion för att visa replikering beräkningstider och senaste replikeringen tid för min sekundära MyDB-databaser.
   
        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status
7. Klicka på **Kör** att köra frågan.
8. Använd följande instruktion om du vill visa de senaste geo-replikering åtgärder som associeras med databasen MyDB.
   
        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC
9. Klicka på **Kör** att köra frågan.

## <a name="next-steps"></a>Nästa steg
* Mer information om växling vid fel grupper och aktiv geo-replikering finns - [redundans grupper](sql-database-geo-replication-overview.md)
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md)

