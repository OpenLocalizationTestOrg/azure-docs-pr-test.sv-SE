---
title: "aaaAzure vägledning för SQL Database-prestandajustering | Microsoft Docs"
description: "Den här artikeln kan hjälpa dig att avgöra vilka service tier toochoose för ditt program. Den rekommenderar även sätt tootune ditt program tooget hello mesta möjliga av Azure SQL Database."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: dd8d95fa-24b2-4233-b3f1-8e8952a7a22b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/09/2017
ms.author: carlrab
ms.openlocfilehash: 2699f755391e94ab488ac1e6acedd30f8aec4488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-performance-in-azure-sql-database"></a>Justera prestanda i Azure SQL Database

Azure SQL Database tillhandahåller [rekommendationer](sql-database-advisor.md) att du kan använda tooimprove prestandan för din databas eller så kan du låta Azure SQL Database [automatiskt anpassa tooyour programmet](sql-database-automatic-tuning.md) och tillämpa ändringarna som förbättrar prestandan för din arbetsbelastning.

Du har inte några tillämpliga rekommendationer och du fortfarande har problem med prestanda, kan du använda följande metoder tooimprove resultaten hello:
1. Öka [tjänstnivåer](sql-database-service-tiers.md) och ger mer resurser tooyour databas.
2. Finjustera ditt program och tillämpa några metoder som kan förbättra prestanda. 
3. Finjustera hello databasen genom att ändra index och frågor toomore effektivt arbeta med data.

Dessa är manuella metoder eftersom du behöver toodecide vad [tjänstnivåer](sql-database-service-tiers.md) väljer du eller om du behöver toorewrite program eller databasen och deply hello ändringar.

## <a name="increasing-performance-tier-of-your-database"></a>Öka prestandanivån för din databas

Azure SQL Database erbjuder fyra [tjänstnivåer](sql-database-service-tiers.md) som du kan välja mellan: Basic, Standard, Premium och Premium-RS (prestanda mäts i dataöverföringsenheter, eller [dtu: er](sql-database-what-is-a-dtu.md). Varje tjänstnivå isolerar strikt hello resurser att din SQL-databas kan använda och garanterar förutsägbar prestanda för den servicenivån. Vi erbjuder vägledning som hjälper dig att välja hello tjänstnivå för ditt program i den här artikeln. Dessutom diskuterar vi sätt att du kan finjustera din programmet tooget hello mesta möjliga av Azure SQL Database.

> [!NOTE]
> Den här artikeln fokuserar på prestanda vägledning för enskilda databaser i Azure SQL Database. Prestandaråd relaterade tooelastic pooler finns [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool-guidance.md). Observera dock att du kan använda många hello justera rekommendationerna i den här artikeln toodatabases i en elastisk pool och få liknande prestandafördelarna.
> 

* **Grundläggande**: hello grundläggande service tier erbjudanden goda prestanda förutsägbarhet för varje databas, timme över timme. I en grundläggande databas stöder tillräckligt med resurser för goda prestanda i en liten databas som inte har flera samtidiga begäranden. Vanliga användningsområden när du använder grundläggande tjänstnivå är:
  * **Du precis har börjat med Azure SQL Database**. Program som är under utveckling ofta behöver inte högpresterande nivåer. Basic-databaser är en perfekt miljö för utveckling eller testning vid ett lågt pris.
  * **Du har en databas med en enda användare**. Program som vanligtvis associerar en användare med en-databas har inte hög kraven för samtidighet och prestanda. Dessa program lämpar sig för hello grundläggande tjänstnivån.
* **Standard**: hello Standard-tjänstnivå erbjuder bättre prestanda, förutsägbarhet och ger bra prestanda för databaser som har flera samtidiga förfrågningar, som arbetsgrupp och webbprogram. När du väljer en Standard-nivån tjänstdatabasen, du kan ändra storlek på databasen appen baserat på förutsägbar prestanda, minut över minut.
  * **Databasen har flera samtidiga förfrågningar**. Program som tjänsten mer än en användare åt gången vanligtvis måste högre prestandanivåer. Exempelvis är arbetsgrupps- eller webbappar program som har låg toomedium-i/o-trafik krav stöder flera samtidiga frågor bra kandidater för hello Standard-tjänstnivå.
* **Premium**: hello premiumnivån erbjuder förutsägbara prestanda, andra över andra, för varje Premium-databas. När du väljer hello premiumnivån storleken du databasprogrammet baserat på hello belastning för den här databasen. hello plan tar bort fall där prestanda varians kan orsaka små frågor tootake längre tid än förväntat i känslig för fördröjningar åtgärder. Den här modellen kan förenkla hello utvecklings- och validering-cykler för program som behöver toomake starkt uttalanden om resursbehov för belastning, prestanda varians eller svarstid. Premium-tjänsten nivå används oftast har en eller flera av följande egenskaper:
  * **Hög belastning**. Ett program som kräver betydande CPU, minne eller in-/ utdata (I/O) toocomplete åtgärderna kräver en dedikerad, högpresterande nivå. Till exempel en databasåtgärd kända tooconsume flera processorkärnor en längre tid är en kandidat för hello premiumnivån.
  * **Många samtidiga förfrågningar**. Vissa databasprogram tjänst många samtidiga förfrågningar, exempelvis när du hanterar en webbplats som har en hög trafik. Basic och Standard tjänstnivåer begränsa hello antalet samtidiga förfrågningar per databas. Program som kräver fler anslutningar måste toochoose en lämplig reservation storlek toohandle hello maxantalet nödvändiga förfrågningar.
  * **Låg latens**. Vissa program behöver tooguarantee svar från hello databasen i minimal tid. Om en specifik lagrad procedur anropas som en del av en bredare kund-åtgärd, kanske du en krav toohave en retur från anropet i fler än 20 millisekunder 99 procent av hello tid. Den här typen av programmet hello premiumnivån fördelar, se till att den hello nödvändiga datorkraft toomake är tillgänglig.
* **Premium-RS**: hello Premium RS nivån är utformad för i/o-intensiva arbetsbelastningar som inte kräver hello högsta garantier för tillgänglighet. Exempel innefattar testning högpresterande arbetsbelastningar eller en analytiska arbetsbelastningar där hello databasen inte är hello system för posten.

hello servicenivå som du behöver för din SQL-databas är beroende av hello högsta belastningen krav för varje resursdimension. Vissa program använder en trivial mängd en enskild resurs, men har betydande krav för andra resurser.

### <a name="service-tier-capabilities-and-limits"></a>Tjänsten funktioner och begränsningar

Vid varje tjänstnivå värdet hello prestandanivå så att du har hello flexibilitet toopay endast för hello kapacitet du behöver. Du kan [justera kapaciteten](sql-database-service-tiers.md), uppåt eller nedåt när arbetsbelastningen ändras. Till exempel kan databasen belastningen är hög under hello tillbaka till skolan perioder som jul du öka hello prestandanivån för hello databas för en viss tid, juli via September. Du kan minska den när belastning-säsongen avslutas. Du kan minimera du betalar genom att optimera molnet miljö toohello säsongsvärdet av ditt företag. Den här modellen fungerar också bra för programvara produkten versionen cykler. Ett test-team kan allokera kapacitet när det testa körs, och släpper den kapaciteten när de är klara testning. I en begäran modell kapacitet betalar du för kapacitet du behöver den och undvika att förlora på dedicerade resurser som du kan använda sällan.

### <a name="why-service-tiers"></a>Varför tjänstnivåer?
Även om varje arbetsbelastning i databasen kan skilja sig, syftar hello tjänstnivåer tooprovide prestanda förutsägbarhet på olika prestandanivåer. Kunder med stora databasen resurskrav kan arbeta i en mer dedikerade datormiljö.

## <a name="tune-your-application"></a>Finjustera ditt program
I traditionella lokala SQL Server, är hello processen för inledande kapacitetsplanering ofta skild från hello processen med att köra ett program i produktion. Maskin- och licenser köps först och prestandajustering görs efteråt. När du använder Azure SQL Database är en bra idé toointerweave hello process för att köra ett program och justera den. Med hello modell för betalar för kapacitet på begäran, kan du finjustera din toouse hello minsta programresurser krävs nu, i stället för överetablering på maskinvara baserat på gissningar framtida tillväxt planer för ett program som ofta är felaktiga. Vissa kunder kan Välj inte tootune ett program och i stället toooverprovision maskinvaruresurser. Den här metoden kan vara en bra idé om du inte vill toochange ett program som nyckel under en period som är upptagen. Men inställning av ett program kan minimera resurskraven och lägre månatliga växlar när du använder hello tjänstnivåer i Azure SQL Database.

### <a name="application-characteristics"></a>Egenskaper för programmet
Men Azure SQL Database servicenivåer är utformad tooimprove prestanda stabilitet och förutsägbarhet för ett program, kan några rekommendationer hjälpa dig justera programmet toobetter dra nytta av hello resurser på prestandanivå. Även om många program har betydande prestandaförbättringar bara av växla tooa högre prestandanivå eller service-nivån, vissa program behöver ytterligare finjustera toobenefit från en högre servicenivå. Överväg att ytterligare finjustera för program som har följande egenskaper för bättre prestanda:

* **Program som har långsam prestanda på grund av ”chatty” beteende**. Chatty program Se stor mängd data access-åtgärder som är känsliga toonetwork svarstid. Du kan behöva toomodify dessa typer av program tooreduce hello antal data access operations toohello SQL-databas. Exempelvis kan du förbättra programmets prestanda genom att använda tekniker som batchbearbetning ad hoc-frågor eller flytta hello frågar toostored procedurer. Mer information finns i [Batch-frågor](#batch-queries).
* **Databaser med en intensiv arbetsbelastning som inte stöds av en enskild dator hela**. Databaser som överskrider hello resurser av hello högsta Premium prestandanivå kan dra nytta av skala ut hello arbetsbelastning. Mer information finns i [flera databaser horisontell partitionering](#cross-database-sharding) och [funktionella partitionering](#functional-partitioning).
* **Program som har något sämre frågor**. Program, särskilt de i hello dataåtkomstnivå, som har dåligt justerade frågor kan inte dra nytta av en högre prestandanivå. Detta inkluderar frågor som saknar en WHERE-sats, saknar index eller vara inaktuell statistik. Dessa program utnyttja standardfråga prestandajustering tekniker. Mer information finns i [saknas index](#identifying-and-adding-missing-indexes) och [fråga justera och bl a](#query-tuning-and-hinting).
* **Program som har något sämre data åt design**. Program som har inbyggd databassamtidighet åtkomstproblem, till exempel deadlocking kan inte dra nytta av en högre prestandanivå. Överväg att minska sändningar mot hello Azure SQL Database av cachelagring på klientsidan för hello med hello Azure Caching service eller en annan teknik för cachelagring. Se [programmet nivån cachelagring](#application-tier-caching).

## <a name="tune-your-database"></a>Finjustera din databas
I det här avsnittet ska titta vi på några metoder som du kan använda tootune Azure SQL Database toogain hello bästa prestanda för ditt program och kör det på hello lägsta möjliga prestandanivån. Vissa av dessa tekniker matchar traditionella SQL Server justera bästa praxis, men andra är särskilda tooAzure SQL-databas. I vissa fall kan du undersöka hello används resurser för en databas toofind områden toofurther finjustera och utöka traditionella SQL Server-teknik toowork i Azure SQL Database.

### <a name="identify-performance-issues-using-azure-portal"></a>Identifiera prestandaproblem med hjälp av Azure portal
hello följande verktyg i hello Azure-portalen kan hjälpa dig att analysera och åtgärda prestandaproblem med SQL-databasen:

* [Query Performance Insight](sql-database-query-performance.md)
* [SQL Database Advisor](sql-database-advisor.md)

hello Azure-portalen har mer information om de här verktygen och hur toouse dem. tooefficiently diagnostisera och åtgärda problem, rekommenderar vi att du först försöka hello verktyg i hello Azure-portalen. Vi rekommenderar att du använder hello manuell justera metoder som sedan diskuterar vi för index och frågejusteringar i särskilda fall saknas.

Mer information om hur du identifierar problem i Azure SQL Database på [prestandaövervakning](sql-database-single-database-monitor.md) artikel.

### <a name="identifying-and-adding-missing-indexes"></a>Identifiera och lägga till index som saknas
Ett vanligt problem i OLTP databasprestanda relaterar toohello fysiska databasdesign. Ofta databasen scheman designas och levereras utan att testa i skala (antingen i belastning eller datavolym). Tyvärr hello prestanda för en frågeplan användas i liten skala men försämras avsevärt under produktion nivå datavolymer. hello vanligaste orsaken till problemet är hello saknas rätt index toosatisfy filter eller andra begränsningar i en fråga. Saknade index manifest som en tabell skanna ofta när ett index seek kan vara tillräckligt.

I det här exemplet använder hello valda frågeplanen en genomsökning när en sökning skulle vara tillräckligt:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![En frågeplan med index som saknas](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL Database kan hjälpa dig att hitta och korrigera vanliga saknas index-villkor. Av DMV: er som är inbyggda i Azure SQL Database titta på frågekompileringar där ett index skulle minska avsevärt hello uppskattade kostnaden toorun en fråga. SQL-databas håller reda på hur ofta varje frågeplan körs vid körning av fråga och spårar hello uppskattad mellanrummet mellan hello köra frågeplan och hello sätt en där index fanns. Du kan använda dessa av DMV: er tooquickly gissning vilka ändringar tooyour fysiska databasdesign kan förbättra övergripande arbetsbelastning kostnaden för en databas och dess faktiska arbetsbelastningen.

Du kan använda den här frågan tooevaluate potentiella saknas index:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

I det här exemplet resulterade hello frågan i den här:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

När den har skapats hämtar en annan plan som använder en Målsökning i stället för en genomsökning och utför hello plan mer effektivt att samma SELECT-uttryck:

![En frågeplan med korrigerade index](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

hello viktiga insikter är att hello i/o-kapacitet för en delad, andra system är mer begränsad än den som en dedikerad server-dator. Det finns en premium på Minimera onödiga i/o tootake maximal nytta av hello system i hello DTU för varje prestandanivå hello Azure SQL Database servicenivåer. Lämplig fysiska databasen designalternativen kan avsevärt förbättra hello svarstid för enskilda frågor, förbättra samtidiga förfrågningar som hanteras per skalningsenhet hello genomflöde och minimera hello kostnader krävs toosatisfy hello frågan. Mer information om hello saknas index av DMV: er finns [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Frågejusteringar och bl a
hello Frågeoptimeringen i Azure SQL Database är liknande toohello-frågeoptimeraren för traditionella SQL Server. De flesta av hello bästa praxis för att finjustera frågor och förstå hello skäl modellen begränsningar för hello frågeoptimeraren även gäller tooAzure SQL-databas. Om du finjustera frågor i Azure SQL Database, kan du få hello fördelen att minska behovet av sammanställda resursdata. Programmet kan vara kan toorun till en lägre kostnad än untuned motsvarande eftersom det kan köras på en lägre prestandanivå.

Ett exempel som är vanliga i SQL Server och som också gäller tooAzure SQL-databas är hur hello fråga optimering ”lyssnar” parametrar. Under kompilering utvärderar hello frågeoptimeraren hello aktuellt värde för en parameter toodetermine om det kan generera en mer optimala frågeplan. Även om den här strategin ofta kan leda tooa frågeplan som är betydligt snabbare än en plan som kompileras utan kända parametervärden för närvarande fungerar imperfectly både i SQL Server och i Azure SQL Database. Hello-parametern är ibland inte någon lyssnar, och ibland hello parametern någon lyssnar men hello genererade planen är något sämre för hello fullständig uppsättning parametervärden i en arbetsbelastning. Microsoft inkluderar frågetips (direktiven) så att du kan ange avsikt mer avsiktligt och åsidosätta hello standardbeteendet för parametern kontroll. Om du använder tips, ofta, kan du åtgärda fall där hello SQL Server eller Azure SQL Database standardbeteendet är ofullständig för en viss kund-arbetsbelastning.

hello nästa exemplet visar hur hello frågeprocessorn kan generera en plan som är något sämre både för prestanda och resurskraven. Det här exemplet visar också att du kan minska Frågekörningen och resursen kraven för SQL-databasen om du använder en frågetipset:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

hello Inställningskod skapar en tabell som har förvrängd Datadistribution. hello optimala frågeprestanda plan skiljer sig baserat på vilken parameter är markerad. Tyvärr kompileras inte hello plan funktionssätt för cachelagring alltid hello fråga baserat på de vanligaste hello-parametervärdet. Det är därför möjligt för en något sämre plan toobe cachelagras och används för många värden, även om ett annat schema kan vara bra plan i genomsnitt. Sedan skapar hello frågeplan två lagrade procedurer som är identiska, förutom att en har en särskild frågetipset.

**Exempel, del 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times tooshow hello performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Exempelvis del 2**

(Vi rekommenderar att du vänta minst 10 minuter innan du börjar del 2 av hello exempelvis så att hello resultat är distinkt i hello resulterande telemetridata.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Varje del av det här exemplet försöker toorun ett parametriserat insert-instruktionen 1 000 gånger (toogenerate en tillräcklig belastningen toouse som en datauppsättning för test). När den körs lagrade procedurer, undersöker hello frågeprocessorn hello parametervärde som skickas toohello proceduren under sin första kompilering (parametern ”identifiering”). hello processor cachelagrar hello i planen och använder den för senare anrop, även om hello parametervärdet är olika. hello optimala plan kan inte användas i samtliga fall. Ibland behöver tooguide hello optimering toopick en plan som är bättre för hello genomsnittlig ärende i stället för hello specifika fall från när hello frågan kompilerades först. I det här exemplet skapar hello ursprungliga planen en ”genomsökning” plan som läser alla rader toofind varje värde som matchar hello parameter:

![Fråga justera med hjälp av en plan för sökning](./media/sql-database-performance-guidance/query_tuning_1.png)

Eftersom vi köra hello proceduren med hjälp av hello värdet 1 hello i planen är optimala för hello värdet 1 men har något sämre för alla andra värden i hello tabell. hello resultatet förmodligen inte vad du kan om du toopick varje planera slumpmässigt eftersom hello plan utför långsammare och använder mer resurser.

Om du kör testet hello med `SET STATISTICS IO` ställa in också`ON`, hello logiska sökning i det här exemplet arbetet hello bakgrunden. Du kan se att det finns 1,148 läsningar av hello plan (vilket är ineffektiv om hello genomsnittlig tooreturn bara en rad):

![Fråga inställning genom att använda en logisk genomsökning](./media/sql-database-performance-guidance/query_tuning_2.png)

hello andra delen av hello exempel används en fråga tipset tootell hello optimering toouse ett specifikt värde under hello kompileringsprocessen. I så fall tvingas hello läsa processor tooignore hello värde skickas som hello parameter och i stället tooassume `UNKNOWN`. Detta refererar tooa-värde som har hello genomsnittlig frekvens i hello tabellen (ignoreras skeva). hello i planen är en plan med seek som är snabbare och använder färre resurser i genomsnitt än hello plan i del 1 i det här exemplet:

![Frågejusteringar med hjälp av en ledtråd för frågan](./media/sql-database-performance-guidance/query_tuning_3.png)

Du kan se hello effekt i hello **sys.resource_stats** tabellen (det finns en fördröjning från hello tid att köra hello test och när hello data fylls hello tabell). För det här exemplet, del 1 körs under tidsperioden för hello 22:25:00 och del 2 utförs 22:35:00. hello används tidigare tidsfönstret mer resurser under den tidsperioden än hello senare version (på grund av planen effektivitet förbättringar).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Prestandajustering exempel frågeresultat](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> Hello volym i det här exemplet är avsiktligt liten kan hello effekten av något sämre parametrarna vara betydande, särskilt på större databaser. hello skillnaden i extrema fall kan vara mellan sekunder för snabb fall och timmar för långsam fall.
> 
> 

Du kan undersöka **sys.resource_stats** toodetermine om hello resurs för ett test använder fler eller färre resurser än en annan test. När du jämför data separata hello tidtagningen av testerna så att de inte ingår i hello samma 5 minuter fönster i hello **sys.resource_stats** vyn. hello är målet hello övningen toominimize hello totala mängden resurser som används och inte toominimize hello belastning resurser. I allmänhet Optimera ett kodavsnitt för fördröjning minskar också risken för förbrukning av nätverksresurser. Se till att hello ändringar tooan program är nödvändiga och att hello ändringar inte påverka hello kundupplevelsen för en person som använder frågetips i hello program.

Om en arbetsbelastning har en uppsättning frågor upprepas, ofta den gör meningsfullt toocapture och validera hello begränsningar av dina val för planen eftersom den styr hello minsta storlek enhet krävs toohost hello resursdatabasen. När du validera det ibland på nytt granska hello planer toohelp du se till att de inte har degraderats. Du kan lära dig mer om [fråga tips (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Flera databaser horisontell partitionering
Eftersom Azure SQL Database körs på vanlig maskinvara, är hello kapacitetsbegränsningar för en enskild databas lägre än för en traditionella lokala SQL Server-installation. Vissa kunder använder databasåtgärder för horisontell partitionering tekniker toospread över flera databaser om hello operations inte passar i hello gränserna för en enskild databas i Azure SQL Database. De flesta kunder som använder tekniker för horisontell partitionering i Azure SQL Database dela sina data på en enda dimension på flera databaser. Du måste toounderstand att OLTP ofta programmens transaktioner som gäller tooonly en rad eller tooa liten grupp av rader i hello schemat för den här metoden.

> [!NOTE]
> SQL-databasen innehåller nu ett bibliotek tooassist med horisontell partitionering. Mer information finns i [översikt över klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md).
> 
> 

Till exempel om en databas har kundnamn, ordning och orderinformationen (t.ex. hello traditionella exempel databasen Northwind som levereras med SQL Server), du kan dela dessa data i flera databaser genom att gruppera en kund med hello relaterade ordning och ordning information information. Du kan garantera hello kundens data förblir i en enskild databas. hello-programmet skulle dela olika kunder på databaser, effektivt sprida hello belastningen över flera databaser. Med horisontell partitionering, kunder inte bara kan undvika hello maximala databasens storleksgräns, men Azure SQL Database kan också bearbeta arbetsbelastningar som är betydligt större än hello gränserna för hello olika prestandanivåer, förutsatt att varje enskild databas passar in i dess DTU.

Horisontell partitionering databasen inte minska hello sammanställda resursdata kapacitet för en lösning, är men högeffektiv stöd för mycket stora lösningar som kan sträcka sig över flera databaser. Varje databas kan köra på en annan prestanda som är nivå toosupport mycket stora ”effektiva” databaser med höga resurskrav.

### <a name="functional-partitioning"></a>Funktionella partitionering
SQL Server-användare kombinera ofta många funktioner i en enskild databas. Om ett program har logik toomanage inventeringen av ett Arkiv, kan till exempel att databasen ha logiken som är associerad med inventering, spårning inköps, lagrade procedurer och indexerade eller materialiserade vyer som hanterar rapportering sista månad. Den här tekniken blir det enklare tooadminister hello databasen för åtgärder som säkerhetskopiering, men kräver också du toosize hello maskinvara toohandle hello belastning över alla funktioner i ett program.

Om du använder en skalbar arkitektur i Azure SQL Database är en bra idé toosplit olika funktioner för ett program till en annan databas. Med den här tekniken kan skalas varje program oberoende av varandra. Som ett program blir busier (och hello belastningen på hello databasen ökar) kan hello-administratören välja oberoende prestandanivåer för varje funktion i hello program. Ett program kan vara större än en enskild vara dator kan hantera eftersom hello belastningen är fördelade på flera datorer på hello gränsen med den här arkitekturen.

### <a name="batch-queries"></a>Batch-frågor
För program som har åtkomst till data med hjälp av stora volymer, ofta ad hoc-frågor till en stor mängd svarstid kan hänföras nätverkskommunikation mellan hello programmet nivå och hello Azure SQL Database-nivå. Även om båda hello är program- och Azure SQL Database i hello samma datacenter, hello Nätverksfördröjningen mellan hello två kan förstoras med ett stort antal dataåtgärder åtkomst. tooreduce hello nätverket avrunda resor för hello data åtkomståtgärder, Överväg att använda hello alternativet tooeither batch hello ad hoc-frågor eller toocompile dem som lagrade procedurer. Om du batch hello ad hoc-frågor kan du skicka flera frågor som en stor batch i en enda resa tooAzure SQL-databas. Om du kompilerar ad hoc-frågor i en lagrad procedur kan du uppnå hello samma resultat som om du batch-dem. Använda en lagrad procedur också ger du hello fördelen med att öka hello risken för cachelagring hello frågeplaner i Azure SQL Database så att du kan använda hello lagrade proceduren igen.

Vissa program är processorintensiva skrivning. Ibland kan du minska hello total i/o-belastningen på en databas genom att beakta hur toobatch skriver tillsammans. Oftast är det enkelt med explicita transaktioner i stället för automatiskt genomförande transaktioner i lagrade procedurer och ad hoc-batchar. En utvärdering av olika metoder som du kan använda finns i [batchbearbetning tekniker för SQL Database-program i Azure](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimentera med din egen arbetsbelastning toofind hello rätt modell för batchbearbetning. Vara säker på att toounderstand som en modell kan ha lite garanterar olika transaktionskonsekvens. Hitta hello rätt arbetsbelastning som minimerar Resursanvändning kräver att hitta hello rätt kombination av konsekvens och prestanda avvägningarna.

### <a name="application-tier-caching"></a>Programmet skikt cachelagring
Vissa databasprogram har läs-frekventa arbetsbelastningarna. Cachelagring lager kan minska hello belastningen på hello-databasen och kan minska nivå krävs toosupport för hello prestanda en databas med hjälp av Azure SQL Database. Med [Azure Redis-Cache](https://azure.microsoft.com/services/cache/), om du har en Läs frekventa arbetsbelastning kan du läsa hello data en gång (eller kanske en gång per program skikt dator, beroende på hur den är konfigurerad), och sedan lagra dessa data utanför din SQL-databas. Detta är ett sätt tooreduce databasbelastningen (processor och Läs i/o), men det finns en effekt på transaktionskonsekvens eftersom hello data som läses från hello cachen kanske är synkroniserad med hello data i hello-databas. Även om vissa andelen inkonsekvens är acceptabel för många program, som gäller inte för alla arbetsbelastningar. Du bör till fullo förstå kraven för application innan du implementerar en strategi för cachelagring av programmet skikt.

## <a name="next-steps"></a>Nästa steg
* Mer information om tjänstnivåer finns [SQL Database-alternativ och prestanda](sql-database-service-tiers.md)
* Läs mer om elastiska pooler [vad är en Azure elastisk pool?](sql-database-elastic-pool.md)
* Information om prestanda och elastiska pooler finns [när tooconsider en elastisk pool](sql-database-elastic-pool-guidance.md)

