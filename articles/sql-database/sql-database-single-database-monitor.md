---
title: aaaMonitoring databasprestanda i Azure SQL Database | Microsoft Docs
description: "Läs mer om hello alternativ för att övervaka din databas med Azure-verktyg och dynamiska hanteringsvyer."
keywords: "databasövervakning, molndatabasprestanda"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: b13771183d4ccf37f58e2fc518b9b14de38212dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Övervaka databasprestanda i Azure SQL Database
Övervakning hello prestanda i en SQL-databas i Azure startar med att övervaka hello resurs användning relativa toohello nivån på databasprestanda som du väljer. Övervakning hjälper dig att avgöra om din databas har överflödig kapacitet eller har problem med eftersom resurserna är överutnyttjade ut och sedan avgöra om det är tid tooadjust hello prestandanivå och [tjänstnivån](sql-database-service-tiers.md) för din databas. Du kan övervaka din databas med grafiska verktyg i hello [Azure-portalen](https://portal.azure.com) eller med hjälp av SQL [dynamiska hanteringsvyer](https://msdn.microsoft.com/library/ms188754.aspx).

## <a name="monitor-databases-using-hello-azure-portal"></a>Övervaka databaser med hello Azure-portalen
I hello [Azure-portalen](https://portal.azure.com/), du kan övervaka en enskild databas resursutnyttjning genom att välja din databas och klicka på hello **övervakning** diagram. Detta öppnar en **mått** fönster som du kan ändra genom att klicka på hello **redigera diagram** knappen. Lägg till hello följande mått:

* CPU-procent
* DTU-procent
* Data IO-procent
* Databasstorlek i procent

När du har lagt till de här måtten, kan du fortsätta tooview dem i hello **övervakning** diagram med mer information om hello **mått** fönster. Alla fyra mätvärdena visar hello genomsnittlig användning procentandel relativa toohello **DTU** för din databas. Se hello [tjänstnivåer](sql-database-service-tiers.md) artikeln för information om dtu: er.

![Tjänstnivå-övervakning av databasprestanda.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Du kan också konfigurera aviseringar på hello prestandamått. Klicka på hello **Lägg till avisering** knapp i hello **mått** fönster. Följ hello guiden tooconfigure aviseringen. Du har hello alternativet tooalert om hello måttet överskrider ett visst tröskelvärde eller om hello måttet faller under ett visst tröskelvärde.

Till exempel om du räknar hello arbetsbelastningen på din databas toogrow du tooconfigure en e-postavisering när databasen når 80% av hello prestandamått. Du kan använda den som en tidig varning toofigure ut när du kanske tooswitch toohello högre prestandanivå.

hello prestandamåtten kan också hjälpa dig att avgöra om du kan toodowngrade tooa lägre prestandanivå. Anta att du använder en Standard S2-databas och alla prestanda mått visar att hello databasen i genomsnitt använder inte mer än 10% vid något tillfälle. Det är troligt att hello-databasen fungerar bra i Standard S1. Dock vara medveten om arbetsbelastningar som varierar kraftigt innan du gör hello beslut toomove tooa lägre prestandanivå.

## <a name="monitor-databases-using-dmvs"></a>Övervaka databaser med hjälp av DMV:er
hello samma mått som exponeras i hello portal finns också tillgängliga via systemvyer: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) i hello logiska **master** databasen på din server och [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) i hello användardatabas. Använd **sys.resource_stats** om du behöver toomonitor mindre detaljerad data över en längre tidsperiod. Använd **sys.dm_db_resource_stats** om du behöver toomonitor mer detaljerad data inom ett kortare tidsintervall. Mer information finns i [Azure SQL Database-prestandaråd](sql-database-single-database-monitor.md#monitor-resource-use).

> [!NOTE]
> **sys.dm_db_resource_stats** returnerar en tom resultatuppsättning när den används i databaser av Webb- och Business-utgåva som är föråldrade.
>
>

### <a name="monitor-resource-use"></a>Övervaka Resursanvändning

Du kan övervaka resurs användning med [SQL Database Query Performance Insight](sql-database-query-performance.md) och [Frågearkivet](https://msdn.microsoft.com/library/dn817826.aspx).

Du kan också övervaka användningen med hjälp av dessa två vyer:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

#### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Du kan använda hello [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) vyn i varje SQL-databas. Hej **sys.dm_db_resource_stats** vyn visar senaste resursen används data relativa toohello tjänstnivån. Genomsnittlig procent för processor, data i/o, skrivs loggen och minne registreras var 15: e sekund och bevaras i timmen.

Eftersom den här vyn innehåller en mer detaljerad titt på Resursanvändning, använda **sys.dm_db_resource_stats** första för alla aktuella tillstånd analys eller felsökning. Den här frågan visar till exempel hello medel och maximala använder för hello databasen via hello senaste timmen:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Andra frågor finns i hello i [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

#### <a name="sysresourcestats"></a>sys.resource_stats
Hej [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) vyn i hello **master** databasen innehåller ytterligare information som kan hjälpa dig att övervaka hello prestanda för SQL-databasen på den specifika tjänstnivå och prestandanivå nivån . hello data samlas var femte minut och underhålls i cirka 35 dagar. Den här vyn är användbar för en långsiktiga historiska analys av hur SQL-databasen använder resurser.

hello följande diagram visar hello CPU resursanvändningen för en Premium-databas med hello P2 prestandanivå för varje timme under en vecka. Det här diagrammet startar på en måndag, visar 5 arbetsdagar och sedan visas en helger, när mycket mindre sker på hello program.

![Resursanvändning för SQL-databas](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Från hello data, har den här databasen för närvarande en CPU-belastning bara över 50% CPU-användning relativa toohello P2 prestandanivå (mitt på dagen tisdagen). Om Processorn är hello företag faktor i hello programmet resurs profil, kan du bestämma att P2 är hello rätt prestanda nivå tooguarantee som hello arbetsbelastning alltid får plats. Om du förväntar dig ett program toogrow över tid är en bra idé toohave en buffert som extra resurs så att programmet hello inte någonsin nå hello prestandanivå gränsen. Om du ökar hello prestandanivå du undvika kunden visas fel som kan uppstå när en databas inte har tillräckligt med power tooprocess begäranden effektivt, särskilt i miljöer med känslig för fördröjningar. Ett exempel är en databas som har stöd för ett program som återger webbsidor utifrån hello resultat av databasanrop.

Andra programtyper av kan tolka hello samma diagram på olika sätt. Till exempel om ett program försöker tooprocess löneuppgifter data varje dag och har hello kan samma diagram, detta kind av ”batchjobb” modell göra att vid prestandanivå P1. hello prestandanivå P1 har 100 Dtu jämfört med too200 dtu: er på hello P2 prestandanivå. hello prestandanivå P1 ger halva hello prestanda för hello P2 prestandanivå. Därför är 50 procent av CPU-användning i P2 lika med 100 procent CPU-användning i P1. Om programmet hello saknar tidsgränser kanske det ingen roll om ett jobb tar 2 timmar eller 2,5 timmar toofinish om den hämtar gjort idag. Ett program i den här kategorin kan förmodligen använda prestandanivå P1. Du kan dra nytta av hello faktum att tidsperioder under hello dag när Resursanvändning är lägre, så att alla ”stor belastning” kan hamnar i en hello tvättställ senare i hello dag. hello prestandanivå P1 kan vara bra för den typen av program (och spara pengar), så länge hello jobb kan avslutas i tid varje dag.

Azure SQL Database visar förbrukas resursinformation för varje aktiv databas i hello **sys.resource_stats** vy över hello **master** databasen i varje server. hello data i hello tabell sammanställs för 5 minuters mellanrum. Med hello Basic, Standard och Premium-servicenivåer, hello data kan det ta mer än 5 minuter tooappear i hello tabell, så att dessa data är mer användbar för historisk analys i stället för analys i nära realtid. Frågan hello **sys.resource_stats** visa toosee hello senaste historiken för en databas och toovalidate om hello reservation du valde levereras hello prestanda när det behövs.

> [!NOTE]
> Du måste vara anslutna toohello **master** databasen för din logiska SQL database server tooquery **sys.resource_stats** i hello följande exempel.
> 
> 

Det här exemplet illustrerar hur hello data i den här vyn visas:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Hej sys.resource_stats katalogvyn](./media/sql-database-performance-guidance/sys_resource_stats.png)

hello nästa exempel visas olika sätt som du kan använda hello **sys.resource_stats** katalogen visa tooget information om hur SQL-databasen använder resurser:

1. toolook på hello tidigare veckans resurs för hello databasen userdb1, kan du köra den här frågan:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. hur väl din arbetsbelastning passar hello prestandanivå tooevaluate, behöver du toodrill ned i varje aspekt av hello resurs mått: processor, läsningar, skrivningar, antalet arbetare och antalet sessioner. Här är en reviderade fråga med **sys.resource_stats** tooreport hello genomsnittliga och högsta värden för de här måtten för resursen:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Med den här informationen om hello genomsnittliga och högsta värden för varje resurs mått bedöma du hur bra din arbetsbelastning passar in i hello prestandanivå du valt. Vanligtvis medelvärden från **sys.resource_stats** ger dig en bra baslinjen toouse mot hello Målstorlek. Det bör vara primär mätning-minne. Exempelvis kan du använda hello Standard-tjänstnivå med S2 prestandanivå. hello medelvärde använder procent för processor- och i/o-läsningar och skrivningar är lägre än 40 procent hello Genomsnittligt antal arbetare understiger 50 och hello Genomsnittligt antal sessioner understiger 200. Din arbetsbelastning kan passa hello S1-prestandanivå. Det är enkelt toosee om databasen passar i hello arbetare och sessionsgränser. toosee om en databas som passar in i en lägre prestandanivå med avseende på tooCPU läsningar och skrivningar, dividera hello DTU antalet hello lägre prestandanivå med hello DTU numret på ditt nuvarande prestandanivå och sedan multiplicera hello resultatet med 100:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    hello resultatet är hello relativa prestandaskillnaden mellan hello två prestandanivåer i procent. Om din Resursanvändning inte överskrider den här mängden, kanske passar din arbetsbelastning i hello lägre prestandanivå. Men du behöver toolook på alla intervall med värden för användning av resursen och i procent avgör hur ofta arbetsbelastningen databasen passar in i hello lägre prestandanivå. hello visar följande fråga hello passar procent per resursdimension, baserat på hello tröskelvärdet 40 procent som vi har beräknats i det här exemplet:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Baserat på din databas servicenivåmål (SLO), kan du bestämma om din arbetsbelastning passar in i hello lägre prestandanivå. Om din databas arbetsbelastning Servicenivåmål är 99,9 procent och hello föregående fråga returnerar värden större än 99,9 procent för alla tre resursdimensioner, din arbetsbelastning som sannolikt passar in i hello lägre prestandanivå.
   
    Titta på hello passar procentandel ger dig även insikt om du ska flytta toohello nästa högre prestanda nivå toomeet din Servicenivåmål. Till exempel visar userdb1 hello efter CPU-användning för hello gångna veckan:
   
   | Genomsnittlig CPU-procent | Högsta CPU-procent |
   | --- | --- |
   | 24.5 |100.00 |
   
    hello Genomsnittlig CPU handlar om ett kvartal av hello gräns på hello prestandanivå, skulle passar in i hello prestandanivå hello-databasen. Men hello maxvärdet visar hello databasen når hello gränsen på hello prestandanivå. Behöver du toomove toohello högre prestandanivå? Titta på hur många gånger din arbetsbelastning når 100 procent och sedan jämföra tooyour databasen arbetsbelastning Servicenivåmål.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Om den här frågan returnerar ett värde mindre än 99,9 procent av hello tre resursdimensioner, Överväg att antingen flytta toohello högre prestandanivå eller använda programmet mottagningsfönster tekniker tooreduce hello belastningen på hello SQL-databas.
4. Den här övningen även tas hänsyn till dina planerade arbetsbelastning ökning hello framtida.

Du kan övervaka enskilda databaser i hello-pool med hello tekniker som beskrivs i det här avsnittet för elastiska pooler. Men du kan också övervaka hello poolen som helhet. Mer information finns i [Övervaka och hantera en elastisk pool](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Högsta antal samtidiga begäranden
toosee hello antalet samtidiga förfrågningar, kör den här Transact-SQL-frågan på din SQL-databas:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

tooanalyze hello arbetsbelastningen på en lokal SQL Server-databasen, ändra den här frågan toofilter på hello viss databas du vill tooanalyze. Till exempel om du har en lokal databas med namnet mindatabas returnerar den här Transact-SQL-frågan hello antal samtidiga begäranden i databasen:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Detta är bara en ögonblicksbild på ett ställe i tid. tooget en bättre förståelse för din arbetsbelastning och krav för antal samtidiga begäranden, behöver du toocollect många prover över tid.

### <a name="maximum-concurrent-logins"></a>Maximal samtidiga inloggningar
Du kan analysera ditt användar- och mönster-tooget en uppfattning om hello frekvensen av inloggningar. Du kan också köra verkliga belastningar i toomake till att du inte träffa detta eller andra begränsningar i den här artikeln diskuterar vi ett test-miljön. Det finns inte en enskild fråga eller dynamisk hanteringsvy (DMV) som kan du visa samtidiga inloggningen räknar eller tidigare.

Om flera klienter använder hello samma anslutningssträng hello service autentiseras varje inloggning. Om 10 användare ansluta samtidigt tooa databasen med hjälp av hello samma användarnamn och lösenord, det skulle vara 10 samtidiga inloggningar. Den här gränsen gäller endast toohello varaktighet hello inloggning och autentisering. Om hello samma 10 användare ansluter sekventiellt toohello databas, skulle hello antal samtidiga inloggningar aldrig vara större än 1.

> [!NOTE]
> Den här begränsningen gäller för närvarande inte toodatabases i elastiska pooler.
> 
> 

### <a name="maximum-sessions"></a>Maximalt antal sessioner
toosee hello antalet aktuella aktiva sessioner, kör den här Transact-SQL-frågan på din SQL-databas:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Ändra hello frågan toofocus på en viss databas om du analyserar en lokal SQL Server-arbetsbelastning. Den här frågan hjälper dig att avgöra en session måste för hello databasen om du funderar på att flytta den tooAzure SQL-databas.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

De här frågorna returnera igen, en tidpunkt i antal. Om du samlar in flera insamlingar över tid, har du hello bästa förståelse för din session använder.

SQL Database-analys, du kan få historiska statistik på sessioner genom att fråga hello [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) visa och granska hello **active_session_count** kolumn. 
