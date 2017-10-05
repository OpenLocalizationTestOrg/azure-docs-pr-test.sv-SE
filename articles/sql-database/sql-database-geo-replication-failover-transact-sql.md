---
title: "TSQL:initiate redundans för Azure SQL Database | Microsoft Docs"
description: "Initiera en planerad eller oplanerad växling för Azure SQL Database med Transact-SQL"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5eb2d256-025d-4f5a-99d4-17f702b37f14
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 459941d2c82e5d4ef62beab4ccf775ab8f5efce4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Starta en planerad eller oplanerad växling för Azure SQL-databas med Transact-SQL

Den här artikeln visar hur du påbörja växling till en sekundär SQL-databas med Transact-SQL. Om du vill konfigurera geo-replikering finns [konfigurera geo-replikering för Azure SQL Database](sql-database-geo-replication-transact-sql.md).

Om du vill initiera växling vid fel, behöver du följande:

* En inloggning som är DBManager på primärt
* Har db_ownership för den lokala databasen som du kommer att geo-replikering
* Vara DBManager på partner-servrar som du vill konfigurera geo-replikering
* Senaste versionen av SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Det rekommenderas att du alltid använder den senaste versionen av Management Studio för att förbli synkroniserad med uppdateringar av Microsoft Azure och SQL Database. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>  

## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Starta en planerad redundans uppgradera en sekundär databas till den nya primärt
Du kan använda den **ALTER DATABASE** instruktionen för att uppgradera en sekundär databas till den nya primära databasen i ett planerat sätt degradering av den befintliga primärt för att bli en sekundär. Den här instruktionen körs i master-databasen på den logiska Azure SQL Database-servern där georeplikerad sekundära databasen som håller på att uppgraderas finns. Den här funktionen är avsedd för planerad redundans exempelvis under DR-flyttar och kräver att den primära databasen är tillgänglig.

Kommandot utför följande arbetsflöde:

1. Växlar tillfälligt replikering till synkront läge orsakar alla utestående transaktioner ska tömmas för sekundärt och blockerar alla nya transaktioner.
2. Växlar roller i de två databaserna i partnerskap geo-replikering.  

Den här sekvensen garanterar att de två databaserna är synkroniserade innan rollerna som växlar och därför ingen data försvinner. Det finns en kort period under vilken båda databaserna är inte tillgänglig (terabyte 0 till 25 sekunder) när rollerna växlas. Om den primära databasen har flera sekundära databaser, konfigurera kommandot automatiskt om de andra sekundärservrar för att ansluta till den nya primärt.  Hela åtgärden bör ta mindre än en minut att slutföra under normala omständigheter. Mer information finns i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) och [tjänstnivåer](sql-database-service-tiers.md).

Använd följande steg för att starta en planerad redundans.

1. Anslut till den logiska Azure SQL Database-servern som en sekundär geo-replikerade databas finns i Management Studio.
2. Öppna mappen databaser, expandera den **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använd följande **ALTER DATABASE** instruktionen för att växla den sekundära databasen till den primära rollen.
   
        ALTER DATABASE <MyDB> FAILOVER;
4. Klicka på **Kör** att köra frågan.

> [!NOTE]
> I sällsynta fall är det möjligt att åtgärden går inte att slutföra och kan visas låsta. I det här fallet kan användaren köra kommando för påtvingad redundans och acceptera data går förlorade.
> 
> 

## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Initiera oplanerad växling från den primära databasen till den sekundära databasen
Du kan använda den **ALTER DATABASE** instruktionen för att uppgradera en sekundär databas till den nya primära databasen på en oplanerad sätt framtvingar degraderingen av den befintliga primärt ska bli en sekundär samtidigt när den primära databasen är inte längre tillgänglig. Den här instruktionen körs i master-databasen på den logiska Azure SQL Database-servern där georeplikerad sekundära databasen som håller på att uppgraderas finns.

Den här funktionen är avsedd för katastrofåterställning när återställning av tillgänglighet för databasen är viktiga och vissa dataförlust är acceptabel. När framtvingad växling vid fel anropas, blir den primära databasen den angivna sekundära databasen omedelbart och börjar ta emot skrivtransaktioner. Så snart som den ursprungliga primära databasen återansluta med den nya primära databasen, utförs en inkrementell säkerhetskopiering på den ursprungliga primära databasen och den gamla primära databasen görs till en sekundär databas för den nya primära databasen; Det är bara en synkronisera repliken av den nya primärt.

Men eftersom punkt i återställning till tidpunkt inte stöds på de sekundära databaserna, om användaren vill återställa data till den gamla primära databasen inte hade har replikerats till den nya primära databasen innan framtvingad växling vid fel inträffade, måste användaren Om du vill delta stöd för att återställa det förlorade data.

Om den primära databasen har flera sekundära databaser, konfigurera kommandot automatiskt om de andra sekundärservrar för att ansluta till den nya primärt.

Använd följande steg för att initiera en oplanerad redundans.

1. Anslut till den logiska Azure SQL Database-servern som en sekundär geo-replikerade databas finns i Management Studio.
2. Öppna mappen databaser, expandera den **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använd följande **ALTER DATABASE** instruktionen för att växla den sekundära databasen till den primära rollen.
   
        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;
4. Klicka på **Kör** att köra frågan.

> [!NOTE]
> Om kommandot utfärdas när både primär och sekundär är online blir den gamla primärt den nya sekundärt omedelbart utan datasynkronisering. Om primärt kan genomför transaktioner när kommandot utfärdas dataförluster uppstå.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Kontrollera autentiseringskraven för servern och databasen har konfigurerats på den nya primärt efter redundans. Mer information finns i [SQL Database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).
* Mer information om återställning efter en katastrof använder aktiv geo-replikering, inklusive före återställning och efter återställning och utför en katastrofåterställning återställningsgranskning finns [Disaster Recovery](sql-database-disaster-recovery.md)
* Ett Sasha Nosov blogginlägg om aktiv geo-replikering finns [Spotlight på nya funktioner för geo-replikering](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
* Information om hur du utformar molnprogram du använder aktiv geo-replikering finns [designa molnprogram för företagskontinuitet med geo-replikering](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* Information om hur du använder aktiv geo-replikering med elastiska pooler finns [elastisk Pool strategi för katastrofåterställning](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
* En översikt över verksamhetskontinuitet finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md)

