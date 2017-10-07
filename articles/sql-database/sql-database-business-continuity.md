---
title: "aaaCloud verksamhetskontinuitet - databasåterställning - SQL-databas | Microsoft Docs"
description: "Lär dig hur Azure SQL Database stöder databasåterställning och affärskontinuitet i molnet och hur du kan köra verksamhetskritiska molnprogram."
keywords: "affärskontinuitet, molnaffärskontinuitet, databashaveriberedskap, databasåterställning"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2017
ms.author: sashan
ms.openlocfilehash: c9a6ff86fbbc04ce839a4fca79594b573b71644c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Översikt över affärskontinuitet med Azure SQL Database

Den här översikten beskriver hello-funktioner som Azure SQL Database tillhandahåller för affärskontinuitet och haveriberedskap. Läs mer om alternativen och rekommendationer självstudier för att återställa från avbrott som kan leda till dataförlust eller leda till din databas och programmet toobecome som är tillgänglig. Lär dig vilka toodo när en användare eller ett program fel påverkar dataintegritet, en Azure-region har ett avbrott eller programmet kräver underhåll.

## <a name="sql-database-features-that-you-can-use-tooprovide-business-continuity"></a>SQL Database-funktioner som du kan använda tooprovide affärskontinuitet

SQL Database tillhandahåller flera funktioner för affärskontinuitet, inklusive automatiserade säkerhetskopieringar och valfri databasreplikering. Den uppskattade återställningstiden (ERT) och den potentiella dataförlusten för de senaste transaktionerna skiljer sig mellan de olika metoderna. Om du förstår de olika alternativen blir det enklare att välja mellan dem, och i de flesta fall kan du använda dem tillsammans för olika scenarier. När du utvecklar en kontinuitetsplan måste toounderstand hello maximal acceptabel tid innan programmet hello fullständigt återställer efter hello störning – det här är din recovery tid mål för Återställningstid. Du måste också toounderstand hello maximal mängd senaste uppdateringar (tidsintervall) hello program kan tolerera förlora när du återställer efter hello störning – det här är ditt mål för återställningspunkt (RPO).

följande tabell jämför hello hello infoga och Återställningspunktmål för hello tre vanligaste scenarierna.

| Funktion | Basic-nivå | Standard-nivå | Premiumnivå |
| --- | --- | --- | --- |
| Återställning till tidpunkt från säkerhetskopia |En återställningspunkt inom 7 dagar |En återställningspunkt inom 35 dagar |En återställningspunkt inom 35 dagar |
| GEO-återställning från säkerhetskopior georeplikerad |ERT < 12 timme, RPO < 1 timme |ERT < 12 timme, RPO < 1 timme |ERT < 12 timme, RPO < 1 timme |
| Återställning från Azure Backup Vault |ERT < 12 timme, RPO < 1 vecka |ERT < 12 timme, RPO < 1 vecka |ERT < 12 timme, RPO < 1 vecka |
| Aktiv geo-replikering |ERT < 30 sekunder, RPO < 5 sekunder |ERT < 30 sekunder, RPO < 5 sekunder |ERT < 30 sekunder, RPO < 5 sekunder |

### <a name="use-database-backups-toorecover-a-database"></a>Använd databas säkerhetskopieringar toorecover en databas

SQL-databas utförs automatiskt en kombination av fullständiga databassäkerhetskopieringar varje vecka, varje timme differentiella säkerhetskopieringar och transaktionen loggsäkerhetskopior var femte-tio minuter tooprotect din verksamhet från förlust av data. Dessa säkerhetskopior lagras i geo-redundant lagring för 35 dagar för databaser i hello Standard och Premium tjänstnivåer och 7 dagar för databaser i hello grundläggande tjänstnivån. Mer information finns i [tjänstnivåer](sql-database-service-tiers.md). Om hello kvarhållningsperiod för din tjänstnivån inte uppfyller företagets krav, kan du öka hello kvarhållningsperiod av [ändra hello tjänstnivån](sql-database-service-tiers.md). hello databas för fullständig och differentiell säkerhetskopiering är också replikerade tooa [parad Datacenter](../best-practices-availability-paired-regions.md) för skydd mot ett avbrott för data center. Mer information finns i [automatiska säkerhetskopieringar](sql-database-automated-backups.md).

Om hello inbyggda kvarhållningsperiod inte är tillräcklig för ditt program, kan du utöka den genom att konfigurera databasen långsiktiga bevarandeprincip. Mer information finns i [långsiktig kvarhållning](sql-database-long-term-retention.md).

Du kan använda dessa automatiska databasen säkerhetskopieringar toorecover en databas från olika störande händelser, både i ditt datacenter och tooanother datacenter. Använda automatiska säkerhetskopieringar hello uppskattade tiden för återställning är beroende av flera faktorer, inklusive hello antalet databaser som återställer hello samma region på hello samma tidpunkt hello databasens storlek hello transaktion loggfilens storlek och nätverkets bandbredd. hello återställningstid är vanligtvis mindre än 12 timmar. När du återställer tooanother dataområdet är hello potentiell dataförlust begränsad too1 timme av hello geo-redundant lagring av varje timme differentiella säkerhetskopieringar.

> [!IMPORTANT]
> toorecover med hjälp av automatisk säkerhetskopiering måste du vara medlem i hello SQL Server deltagarrollen eller hello prenumerationsägaren - Se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md). Du kan återställa med hello Azure-portalen, PowerShell eller hello REST API. Du kan inte använda Transact-SQL.
>

Använd automatiska säkerhetskopieringar som din affärskontinuitets- och återställningsmetod om ditt program:

* Inte betraktas som verksamhetskritiskt.
* Har en bindning SLA - ett driftstopp på 24 timmar eller resulterar längre inte i finansiella ansvar.
* Har ett lågt värde för data ändras (låg transaktioner per timme) och att förlora in tooan timmes ändringen är en godkänd dataförlust.
* Är kostnadskänsligt.

Om du behöver snabbare återställning använder [aktiv geo-replikering](sql-database-geo-replication-overview.md) (beskrivs nedan). Om du behöver toobe kan toorecover data från en period som är äldre än 35 dagar använda [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-in-preview-tooreduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Använd aktiv geo-replikering och automatisk redundans grupper (i förhandsversion) tooreduce recovery tid och gränsen förlust av data som är associerade med en återställning

Dessutom toousing säkerhetskopieringar för återställning av databas om ett företag avbrott inträffar kan du använda [aktiv geo-replikering](sql-database-geo-replication-overview.md) tooconfigure en databas toohave av toofour läsbara sekundära databaser i hello områden av din val av. Dessa sekundära databaser hålls synkroniserade med hello primära databasen med hjälp av en mekanism för asynkron replikering. Den här funktionen är används tooprotect mot avbrott i verksamheten om en data center strömavbrott eller under en uppgradering av programmet. Aktiv geo-replikering kan också vara används tooprovide bättre frågeprestanda för skrivskyddade frågor toogeographically spridda användare.

tooenable automatiskt och transparent redundans du organisera dina geo-replikerade databaser i grupper med hello [automatisk redundans grupp](sql-database-geo-replication-overview.md) funktion i SQL-databas (i förhandsversion).

Om hello primära databasen försätts offline oväntat eller tootake måste det offline för underhållsaktiviteter du kan snabbt befordra en sekundär toobecome hello primär (kallas även en växling vid fel) och konfigurera program tooconnect toohello upphöja primär. Om programmet ansluter toohello databaser med hjälp av hello redundans lyssnare, behöver du inte toochange hello SQL-sträng anslutningskonfiguration efter växling vid fel. Vid en planerad redundansväxling förekommer ingen dataförlust. Med en oplanerad redundans kan det finnas en liten mängd dataförlust för mycket färsk transaktioner på grund av toohello karaktär av asynkron replikering. Med automatisk redundans grupper (i förhandsversion) kan anpassa du hello redundans princip toominimize hello potentiell dataförlust. Efter en redundansväxling kan du senare återställa - bl.a tooa plan eller när hello Datacenter är online igen. I samtliga fall måste användarna uppleva en liten mängd driftstopp och måste tooreconnect.

> [!IMPORTANT]
> toouse aktiv geo-replikering och automatisk redundans grupper (i förhandsversion), du måste antingen vara hello prenumerationsägaren eller har administrativ behörighet i SQL Server. Du kan konfigurera och växla över med hello Azure-portalen PowerShell eller hello REST-API med hjälp av Azure-Prenumerationsbehörigheter eller Transact-SQL med SQL Server-behörighet.
> 

Använd aktiv geo-replikering och automatisk redundans grupper (under förhandsgranskning) om appen uppfyller någon av dessa villkor:

* Är verksamhetskritiskt.
* Har ett serviceavtal (SLA) som inte tillåter driftavbrott på 24 timmar eller mer.
* Driftstopp kan resultera i finansiella ansvar.
* Har en hög frekvens av dataändringar och en timmes dataförlust är inte acceptabelt.
* hello extra kostnad för aktiv geo-replikering är lägre än hello potentiella ekonomiska ansvar och associerade förlust av affärsinformation.

>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Återställa en databas efter ett användar- eller programfel
* Ingen är perfekt! En användare kan oavsiktligt ta bort vissa data eller av misstag radera en viktig tabell eller till och med en hel databas. Eller ett program av misstag kan skriva över bra data med felaktiga data på grund av fel i tooan program.

I det här scenariot har du följande återställningsalternativ.

### <a name="perform-a-point-in-time-restore"></a>Utföra en återställning till en viss tidpunkt
Du kan använda hello automatisk säkerhetskopiering toorecover en kopia av din databas tooa känd bra punkt i tiden, förutsatt att tiden är inom kvarhållningsperioden för hello databasen. När hello databasen återställs kan du ersätta hello originaldatabasen med hello återställs databasen eller kopiera hello behövs data från hello återställa data till hello originaldatabasen. Om hello databasen använder aktiv geo-replikering, rekommenderar vi kopiera hello krävs data från hello återställts kopierar till hello originaldatabasen. Om du ersätter hello originaldatabasen med hello återställs databasen behöver tooreconfigure och synkronisera om aktiv geo-replikering (vilket kan ta tid för en stor databas).

Mer information och detaljerade anvisningar för att återställa en databas tooa punkt i gång med hjälp av hello Azure-portalen eller med hjälp av PowerShell, se [point-in-time-återställning](sql-database-recovery-using-backups.md#point-in-time-restore). Du kan inte återställa med hjälp av Transact-SQL.

### <a name="restore-a-deleted-database"></a>Återställa en borttagen databas
Om hello databasen tas bort men hello logisk server inte har tagits bort, kan du återställa hello bort databasen toohello punkt då den togs bort. Detta återställer en databas säkerhetskopiering toohello samma logiska SQLServer som den togs bort. Du kan återställa den med hjälp av hello ursprungliga namnet eller ange ett nytt namn eller hello återställa databasen.

Mer information och detaljerade anvisningar för att återställa en borttagen databas med hjälp av hello Azure-portalen eller med hjälp av PowerShell, se [återställa en borttagen databas](sql-database-recovery-using-backups.md#deleted-database-restore). Du kan inte återställa med hjälp av Transact-SQL.

> [!IMPORTANT]
> Du kan inte återställa en borttagen databas om hello logisk server tas bort.
>
>

### <a name="restore-from-azure-backup-vault"></a>Återställning från Azure Backup Vault
Du kan återställa från en veckovis säkerhetskopiering i Azure-Säkerhetskopieringsvalvet tooa ny databas om hello dataförlust inträffat utanför hello aktuella kvarhållningsperiod för automatisk säkerhetskopiering och databasen har konfigurerats för långsiktig kvarhållning. Nu kan du antingen ersätta hello originaldatabasen med hello återställs databasen eller kopiera hello behövs data från hello återställs databasen till hello originaldatabasen. Om du behöver tooretrieve en gammal version av databasen tidigare tooa större program uppgraderingen kan uppfylla en begäran från granskare eller en juridiska ordning du kan skapa en databas med en fullständig säkerhetskopia som sparats i hello Azure Backup-valvet.  Mer information finns i avsnittet om [långsiktig kvarhållning](sql-database-long-term-retention.md).

## <a name="recover-a-database-tooanother-region-from-an-azure-regional-data-center-outage"></a>Återställa en databas tooanother region från ett avbrott i Azure regionala data center
<!-- Explain this scenario -->

Även om det är ovanligt finns risken för ett avbrott på ett Azure-datacenter. När ett avbrott uppstår orsakar det ett verksamhetsavbrott som kan vara några få minuter eller flera timmar.

* Ett alternativ är toowait för din databas toocome online igen när hello data center avbrott är över. Detta fungerar för program som har råd toohave hello databasen i offlineläge. Till exempel en utvecklingsprojekt eller kostnadsfri utvärderingsversion behöver du inte toowork på hela tiden. När ett datacenter har ett avbrott, vet du inte hur länge kan det senaste hello avbrott, så det här alternativet fungerar bara om du inte behöver databasen för en stund.
* Ett annat alternativ är tooeither misslyckas över tooanother dataområdet om du använder aktiv geo-replikering eller hello återställer en databas med geo-redundant databassäkerhetskopieringar (geo-återställning). Redundans tar endast några sekunder medan databasåterställning från säkerhetskopior tar timmar.

När du utför en åtgärd, hur lång tid det tar du toorecover och hur mycket du medföra dataförluster beror på hur du bestämmer dig för toouse de här funktionerna för verksamhetskontinuitet i ditt program. Faktiskt du toouse en kombination av säkerhetskopiorna av databasen och aktiv geo-replikering beroende på dina krav för programmet. Information om designöverväganden för fristående databaser och för elastiska pooler när dessa funktioner för affärskontinuitet används finns i [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) (Utforma ett program för haveriberedskap i molnet) och [Elastic Pool disaster recovery strategies](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) (Haveriberedskapsstrategier med en elastisk pool).

hello innehåller följande avsnitt en översikt över hello steg toorecover med säkerhetskopiorna av databasen eller aktiv geo-replikering. Detaljerade anvisningar för planering krav och steg för återställning av post information om hur toosimulate ett avbrott tooperform en katastrofåterställning detaljgranska finns [återställa en SQL-databas från ett avbrott](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Förbereda för ett avbrott
Oavsett hello business continuity funktionen du använder måste du:

* Identifiera och förbereda hello målservern, inklusive brandväggsregler på servernivå, inloggningar och behörigheter för master-databasen på.
* Avgör hur tooredirect klienter och klienten program toohello ny server
* Dokumentera andra beroenden, till exempel granskningsinställningar och aviseringar.

Om du inte framställer korrekt, att dina program online efter en växling vid fel eller en databasåterställning tar extra tid och kan också kräva felsökning i taget stress - en felaktig kombination.

### <a name="fail-over-tooa-geo-replicated-secondary-database"></a>Växla över tooa georeplikerad sekundära databas
Om du använder aktiv geo-replikering och automatisk redundans grupper (i förhandsversion) som återställningsmekanism för kan du konfigurera en princip för automatisk redundans eller använda [manuell växling](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-database). Efter hello redundans orsaker hello sekundära toobecome hello nya primära och redo toorecord nya transaktioner och svara tooqueries - med minimal dataförlust för hello data som inte har replikerats. Information om hur du utformar hello failover-processen finns [utforma ett program för katastrofåterställning i molnet](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> När hello Datacenter är tillbaka online återansluta toohello nya primära hello gamla primärfärgerna automatiskt och blir sekundära databaser. Om du behöver toorelocate hello primära tillbaka toohello ursprungliga region, du kan starta en planerad redundans manuellt (återställning). 
> 

### <a name="perform-a-geo-restore"></a>Utför en geo-återställning
Om du använder automatisk säkerhetskopiering med geo-redundant lagringsreplikering som återställningsmekanism för [initiera en databasåterställning med geo-återställning](sql-database-disaster-recovery.md#recover-using-geo-restore). Återställning sker vanligtvis inom 12 timmar - med dataförlust för in tooone timme bestäms av när hello senaste timvis differentiell säkerhetskopiering med vidtas och replikeras. Tills återställningen hello hello databasen är toorecord alla transaktioner eller också svarar tooany frågor. När detta återställer en databas toohello senaste tillgängliga punkt i tiden, stöds återställa hello geo sekundär tooany punkt i tiden inte för närvarande.

> [!NOTE]
> Om hello Datacenter är online igen innan du växlar ditt program via toohello återställda databasen, kan du avbryta hello återställning.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Utföra åtgärder efter en redundansväxling eller återställning
Du måste utföra hello följande ytterligare uppgifter för din användare och program säkerhetskopiera och körs efter återställningen från antingen funktion:

* Omdirigera klienter och klienten program toohello ny server och återställda databas
* Se till att lämpliga servernivå brandväggsregler finns för användare tooconnect (eller använda [databasnivå brandväggar](sql-database-firewall-configure.md#creating-and-managing-firewall-rules))
* Se till att rätt inloggningar och behörigheter på huvuddatabasnivå är på plats (eller använd [inneslutna användare](https://msdn.microsoft.com/library/ff929188.aspx))
* Konfigurera granskning efter behov
* Konfigurera aviseringar efter behov

## <a name="upgrade-an-application-with-minimal-downtime"></a>Uppgradera ett program med minimal avbrottstid
Ibland måste ett program vara offline på grund av planerat underhåll, till exempel en uppgradering av programmet. [Hantera programuppgraderingar](sql-database-manage-application-rolling-upgrade.md) beskriver hur toouse aktiv geo-replikering tooenable rullande uppgraderingar molnet programmet toominimize driftstopp under uppgraderingar och ange en Återställningssökväg om något går fel. 

## <a name="next-steps"></a>Nästa steg
En beskrivning av designöverväganden för fristående databaser och för elastiska pooler finns i [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) (Utforma ett program för haveriberedskap i molnet) och [Elastic Pool disaster recovery strategies](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) (Haveriberedskapsstrategier med en elastisk pool).
