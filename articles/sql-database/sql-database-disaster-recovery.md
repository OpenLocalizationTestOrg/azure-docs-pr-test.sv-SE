---
title: "aaaSQL databasen katastrofåterställning | Microsoft Docs"
description: "Lär dig hur toorecover en databas från en regionala datacenter avbrott eller ett fel med hello Azure SQL Database aktiv geo-replikering och geo-återställning funktioner."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 4800960e-3f9d-40ce-9e55-fb7f2784c067
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/14/2017
ms.author: sashan
ms.openlocfilehash: bae08485863067748107ec4808e52d8e88e2de0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-database-or-failover-tooa-secondary"></a>Återställa en Azure SQL Database eller failover tooa sekundär
Azure SQL Database ger följande funktioner för återställning efter ett avbrott hello:

* [Aktiv geo-replikering](sql-database-geo-replication-overview.md)
* [GEO-återställning](sql-database-recovery-using-backups.md#point-in-time-restore)

toolearn om kontinuitet affärsscenarier och hello-funktioner som stöder dessa scenarier finns [företagskontinuitet](sql-database-business-continuity.md).

### <a name="prepare-for-hello-event-of-an-outage"></a>Förbereda för hello händelse av ett avbrott
För att lyckas med recovery tooanother dataområdet använder aktiv geo-replikering eller geo-redundant säkerhetskopior behöver du tooprepare en server i en annan data center avbrott toobecome hello nya primära servern bör hello måste uppstå samt ha väldefinierade steg dokumenterade och testade tooensure en mjuk återställning. Dessa förberedelsesteg inkluderar:

* Identifiera hello logisk server i en annan region toobecome hello nya primära servern. Detta kommer att minst en och kanske var och en av hello sekundära servrar med aktiv geo-replikering. För geo-återställning, kommer detta vanligtvis att en server i hello [parad region](../best-practices-availability-paired-regions.md) för hello region där databasen är placerad.
* Identifiera och du kan också definiera, hello brandväggsregler på servernivå behövs på för användare tooaccess hello nya primära databasen.
* Bestämma hur du kommer tooredirect användare toohello nya primära servern, exempelvis genom att ändra anslutningssträngar eller genom att ändra DNS-poster.
* Identifiera, och du kan också skapa, hello inloggningar som måste finnas i hello master-databasen på hello nya primära servern och se till att dessa inloggningar har rätt behörighet i hello master-databasen, om sådana. Mer information finns i [SQL Database-säkerhet efter återställning](sql-database-geo-replication-security-config.md)
* Identifiera Varningsregler som behöver toobe uppdaterade toomap toohello nya primära databasen.
* Dokumentet hello granskning konfigurationen på hello aktuella primära databasen
* Utföra en [disaster recovery ökad](sql-database-disaster-recovery-drills.md). toosimulate ett avbrott för geo-återställning, du kan ta bort eller byta namn på hello källa databasen toocause anslutning programfel. toosimulate ett avbrott för aktiv geo-replikering, kan du inaktivera hello webbprogram eller virtuella datorn är ansluten toohello databas eller failover hello databasen toocause anslutning programfel.

## <a name="when-tooinitiate-recovery"></a>När tooinitiate återställning
hello återställningsåtgärden påverkar hello program. Det innebär att du ändrar hello SQL-anslutningssträng eller genom att använda DNS och kan leda till förlust av data permanent. Det bör därför göras endast när hello avbrott är sannolikt toolast som är längre än mål för ditt program. När hello program är distribuerade tooproduction bör du utför regelbundet för hello programmets hälsotillstånd och använder hello följande punkter tooassert som hello recovery garanteras data:

1. Permanent anslutningsfel från hello nivå toohello databas.
2. hello Azure-portalen visar en avisering om en incident i hello region med bred påverkan.
3. hello Azure SQL Database-server markeras som försämrad.

Beroende på dina program tolerans toodowntime och möjliga business ansvar du hello följande återställningsalternativ för.

Använd hello [hämta återställningsbara databasen](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) tooget hello senaste georeplikerad återställningspunkt.

## <a name="wait-for-service-recovery"></a>Vänta tills tjänsten återställning
hello arbetar Azure ordentligt toorestore tjänsttillgänglighet så snabbt som möjligt, men beroende på hello grundorsaken kan det ta timmar eller dagar.  Om ditt program kan tolerera betydande driftstopp kan du bara vänta hello recovery toocomplete. I så fall krävs ingen åtgärd. Du kan se hello aktuell status för tjänsten på vår [Azure Hälsoinstrumentpanelen](https://azure.microsoft.com/status/). Programmets tillgänglighet kommer att återställas efter hello återställning av hello region.

## <a name="fail-over-toogeo-replicated-secondary-database"></a>Växla över toogeo replikerad sekundära databas
Om ditt programs driftstopp kan resultera i företag ansvar bör du använda geo-replikerade databaser i ditt program. Det gör att programmet hello tooquickly återställning tillgänglighet i en annan region vid ett strömavbrott. Lär dig hur för[konfigurera geo-replikering](sql-database-geo-replication-portal.md).

toorestore tillgängligheten för hello database(s) du behöver tooinitiate hello redundans toohello georeplikerad sekundära med någon av metoderna hello stöds.

Använd någon av hello följande guider toofail över tooa georeplikerad sekundär databas:

* [Växla över tooa georeplikerad sekundära med hjälp av Azure Portal](sql-database-geo-replication-portal.md)
* [Redundans tooa georeplikerad sekundära med hjälp av PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
* [Växla över tooa georeplikerad sekundära med T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Återställa med geo-återställning
Om ditt program driftstopp inte resulterar i business ansvar kan du använda [geo-återställning](sql-database-recovery-using-backups.md) som en metod toorecover program-databaserna. En kopia av hello databas skapas från den senaste geo-redundant säkerhetskopian.

## <a name="configure-your-database-after-recovery"></a>Konfigurera databasen efter återställningen
Om du använder geo-replikering, redundans eller toorecover geo-återställning efter ett avbrott måste du se till att hello anslutningen toohello nya databaser är korrekt konfigurerad så att hello normal programmet funktionen kan återupptas. Detta är en checklista för uppgifter tooget produktionsmiljön återställda databasen redo.

### <a name="update-connection-strings"></a>Uppdatera anslutningssträngar
Eftersom den återställda databasen kommer att finnas i en annan server, måste tooupdate programmets sträng toopoint toothat anslutningsserver.

Mer information om hur du ändrar anslutningssträngar finns hello lämpliga programmeringsspråk för din [anslutningsbibliotek](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Konfigurera brandväggsregler
Du måste toomake att hello brandväggsregler konfigurerat på servern och på hello-databasen matchar de som har konfigurerats på hello primära servern och den primära databasen. Mer information finns i [så här: konfigurera brandväggsinställningar (Azure SQL Database)](sql-database-configure-firewall-settings.md).

### <a name="configure-logins-and-database-users"></a>Konfigurera inloggning och databasanvändare
Du måste toomake till att alla hello inloggningar som används av programmet finns på hello-server som är värd för den återställda databasen. Mer information finns i [säkerhetskonfiguration för geo-replikering](sql-database-geo-replication-security-config.md).

> [!NOTE]
> Du bör konfigurera och testa din serverbrandväggsreglerna och inloggningar (och deras behörigheter) under en katastrofåterställning återställningsgranskning. Dessa servernivå objekt och deras konfiguration får inte vara tillgängliga under hello avbrott.
> 
> 

### <a name="setup-telemetry-alerts"></a>Ställa in telemetri aviseringar
Du måste toomake till de befintliga inställningarna för varningsregeln är uppdaterade toomap toohello återställda databasen och hello annan server.

Mer information om databasen Varningsregler finns [ta emot aviseringen meddelanden](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) och [spåra tjänstens hälsa](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Aktivera granskning
Om granskning är obligatoriska tooaccess databasen, behöver du tooenable granskning efter hello databasåterställning. Mer information finns i [Database auditing](sql-database-auditing.md).

## <a name="next-steps"></a>Nästa steg
* toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md)
* toolearn om kontinuitet design- och affärsscenarier, se [kontinuitet scenarier](sql-database-business-continuity.md)
* toolearn om hur du använder automatisk säkerhetskopiering för återställning finns [återställa en databas från hello service-initierad säkerhetskopiering](sql-database-recovery-using-backups.md)

