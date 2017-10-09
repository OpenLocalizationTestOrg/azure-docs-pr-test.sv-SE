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
ms.openlocfilehash: 418953e044ba84ce758063d56a371af45d5cdfe1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Starta en planerad eller oplanerad växling för Azure SQL-databas med Transact-SQL

Den här artikeln beskrivs hur du tooinitiate redundans tooa sekundär SQL-databas med Transact-SQL. tooconfigure geo-replikering, se [konfigurera geo-replikering för Azure SQL Database](sql-database-geo-replication-transact-sql.md).

tooinitiate redundans, behöver du hello följande:

* En inloggning som DBManager på hello primära
* Har db_ownership av hello lokal databas som du kommer att geo-replikering
* Vara DBManager på hello partner servrar toowhich konfigurerar du geo-replikering
* Senaste versionen av SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Det rekommenderas att du alltid använder hello senaste versionen av Management Studio tooremain synkroniseras med uppdateringar tooMicrosoft Azure och SQL-databas. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>  

## <a name="initiate-a-planned-failover-promoting-a-secondary-database-toobecome-hello-new-primary"></a>Starta en planerad redundans befordrar en ny primär för sekundära databasen toobecome hello
Du kan använda hello **ALTER DATABASE** instruktionen toopromote en sekundär databas toobecome hello nya primära databasen i ett planerat sätt degradering hello befintliga primära toobecome en sekundär. Den här instruktionen körs på hello master-databasen på hello Azure SQL Database logisk server i vilken hello georeplikerad sekundär databas som håller på att uppgraderas finns. Den här funktionen är avsedd för planerad redundans, som under hello DR flyttar och kräver att hello primära databasen är tillgänglig.

hello-kommando utför hello följande arbetsflöde:

1. Tillfälligt rensade växlar replikering toosynchronous läge orsakar alla utestående transaktioner toobe toohello sekundära och blockerar alla nya transaktioner.
2. Växlar hello roller i hello två databaserna hello geo-replikering tillsammans.  

Den här sekvensen garanterar att hello två databaserna är synkroniserade innan hello roller växla och därför ingen data försvinner. Det finns en kort period under vilken båda databaserna är inte tillgängliga (på hello ordning 0 too25 sekunder) när hello roller växlas. Om hello primära databasen har flera sekundära databaser, hello kommandot kommer automatiskt reconfigure hello andra sekundärservrar tooconnect toohello nya primära servern.  hello hela åtgärden bör ta mindre än en minut toocomplete under normala omständigheter. Mer information finns i [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) och [tjänstnivåer](sql-database-service-tiers.md).

Använd hello följande steg tooinitiate en planerad redundans.

1. Anslut toohello Azure SQL Database logiska servern som en sekundär geo-replikerade databas finns i Management Studio.
2. Öppna databasmappen hello, expandera hello **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använder följande hello **ALTER DATABASE** instruktionen tooswitch hello sekundära databasen toohello primära rollen.
   
        ALTER DATABASE <MyDB> FAILOVER;
4. Klicka på **kör** toorun hello frågan.

> [!NOTE]
> I sällsynta fall är det möjligt att hello åtgärden går inte att slutföra och kan visas låsta. I det här fallet kan hello användare köra hello-kommando för påtvingad redundans och acceptera data går förlorade.
> 
> 

## <a name="initiate-an-unplanned-failover-from-hello-primary-database-toohello-secondary-database"></a>Initiera oplanerad växling från hello primära databasen toohello sekundära databas
Du kan använda hello **ALTER DATABASE** instruktionen toopromote en sekundär databas toobecome hello nya primära databasen i ett oplanerad sätt att tvinga hello degraderingen av hello befintliga primära toobecome en sekundär samtidigt när hello primära databasen är inte längre tillgänglig. Den här instruktionen körs på hello master-databasen på hello Azure SQL Database logisk server i vilken hello georeplikerad sekundär databas som håller på att uppgraderas finns.

Den här funktionen är avsedd för katastrofåterställning när återställning tillgängligheten för hello databasen är viktiga och dataförluster tillåts. När framtvingad växling vid fel anropas angetts hello sekundära databasen omedelbart blir hello primära databasen och börjar ta emot skrivtransaktioner. Så snart hello ursprungliga primära databasen är kan tooreconnect med den nya primära databasen, utförs en inkrementell säkerhetskopia på hello ursprungliga primära databasen och hello gamla primära databasen görs till en sekundär databas för hello nya primära databasen. Det är bara en synkronisera repliken av hello nya primära.

Men eftersom punkt i återställning till tidpunkt inte stöds på hello sekundära databaser, om hello som toorecover data allokerat replikerats toohello gamla primära databasen som inte hade har toohello nya primära databasen innan hello framtvingad växling vid fel uppstod hello användaren måste tooengage stöd toorecover detta förlorade data.

Om hello primära databasen har flera sekundära databaser, hello kommandot kommer automatiskt reconfigure hello andra sekundärservrar tooconnect toohello nya primära servern.

Använd följande steg tooinitiate oplanerad växling hello.

1. Anslut toohello Azure SQL Database logiska servern som en sekundär geo-replikerade databas finns i Management Studio.
2. Öppna databasmappen hello, expandera hello **systemdatabaser** mappen, högerklicka på **master**, och klicka sedan på **ny fråga**.
3. Använder följande hello **ALTER DATABASE** instruktionen tooswitch hello sekundära databasen toohello primära rollen.
   
        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;
4. Klicka på **kör** toorun hello frågan.

> [!NOTE]
> Om hello utfärdas när både primär och sekundär är online hello gamla primära blir hello nya sekundära omedelbart utan datasynkronisering. Om hello primära kan genomför transaktioner när hello-kommandot har utfärdats dataförluster uppstå.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Kontrollera hello autentiseringskraven för servern och databasen har konfigurerats på hello nya primära efter redundans. Mer information finns i [SQL Database-säkerhet efter katastrofåterställning](sql-database-geo-replication-security-config.md).
* toolearn återställning efter en katastrof använder aktiv geo-replikering, inklusive före återställning och efter återställning och utför en katastrofåterställning återställningsgranskning finns [Disaster Recovery](sql-database-disaster-recovery.md)
* Ett Sasha Nosov blogginlägg om aktiv geo-replikering finns [Spotlight på nya funktioner för geo-replikering](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
* Information om hur du utformar molnet program toouse aktiv geo-replikering finns [designa molnprogram för företagskontinuitet med geo-replikering](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* Information om hur du använder aktiv geo-replikering med elastiska pooler finns [elastisk Pool strategi för katastrofåterställning](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
* En översikt över verksamhetskontinuitet finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md)

