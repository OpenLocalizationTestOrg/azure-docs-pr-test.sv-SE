---
title: "Databasen kompatibilitetsnivå 130 - Azure SQL Database | Microsoft Docs"
description: "I den här artikeln förklarar vi fördelarna med att köra Azure SQL Database på kompatibilitetsnivå 130 och utnyttja fördelarna med den nya frågeoptimeraren och fråga processorfunktioner. Vi tar också upp möjliga sidoeffekter på frågeprestanda för de befintliga SQL-program."
services: sql-database
documentationcenter: 
author: alainlissoir
manager: jhubbard
editor: 
ms.assetid: 8619f90b-7516-46dc-9885-98429add0053
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.topic: article
ms.date: 08/08/2016
ms.author: alainl
ms.openlocfilehash: c08c0690df4f389416e4ed2e2df2dbb72d6fd567
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Förbättrad prestanda för frågor med nivå 130 i Azure SQL Database
Azure SQL Database körs transparent hundratals tusentals databaser på många olika kompatibilitetsnivåer bevara och säkerställa bakåtkompatibilitet med motsvarande version av Microsoft SQL Server för alla kunder!

I den här artikeln förklarar vi fördelarna med att köra din Azure SQL-Databse på kompatibilitetsnivå 130 och utnyttja fördelarna med den nya frågeoptimeraren och fråga processorfunktioner. Vi tar också upp möjliga sidoeffekter på frågeprestanda för de befintliga SQL-program.

Som en påminnelse om historik justeringen för SQL-versioner till standard kompatibilitetsnivåer är följande:

* 100: i SQL Server 2008 och Azure SQL Database V11.
* 110: i SQL Server 2012 och Azure SQL Database V11.
* 120: i SQL Server 2014 och Azure SQL Database V12.
* 130: i SQL Server 2016 och Azure SQL-databasen (V12).

> [!IMPORTANT]
> Från och med **mid juni 2016**, i Azure SQL Database kompatibilitetsnivån standard blir 130 i stället för 120 för **nyskapad** databaser.
> 
> Databaser som skapats före mitten av juni 2016 kommer *inte* påverkas och kommer bibehålla sina aktuella kompatibilitetsnivån (100, 110 eller 120). Databaser som migrerats från Azure SQL Database version V11 till V12 har en kompatibilitetsnivå 100 eller 110. 
> 

## <a name="about-compatibility-level-130"></a>Om kompatibilitetsnivåer 130
Först om du vill veta den aktuella kompatibilitetsnivån för din databas kör du följande Transact-SQL-sats.

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Innan den här ändringen till nivå 130 sker för **nyligen** skapat databaser, vi gå igenom den här ändringen handlar om till exempel grundläggande frågan och se hur vem som helst kan dra nytta av den.

Frågebearbetning i relationsdatabaser kan vara mycket komplex och kan leda till många datorvetenskap, räkning att förstå inbyggd designalternativen och beteenden. I det här dokumentet har innehållet avsiktligt förenklats så att alla med vissa minsta teknisk bakgrund förstå effekten av ändringen kompatibilitet nivå och ta reda på hur det kan göra program.

Vi har en titt på kompatibilitetsnivån 130 ger på tabellen.  Du hittar mer information i [ändra DATABASENS kompatibilitetsnivån (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), men här är en kort sammanfattning:

* Insert-åtgärden för en Insert select-instruktion kan vara flertrådade eller en parallell plan medan innan den här åtgärden var Enkeltrådig.
* Optimerad tabell och tabellen variabler frågor kan nu använda parallella planer minne medan innan den här åtgärden även var Enkeltrådig.
* Statistik för Minnesoptimerade tabellen kan nu samlas in och kan uppdateras automatiskt. Se [vad är nytt i Database Engine: Minnesintern OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) för mer information.
* Batch-läge v/s radläge ändras med kolumnen index
  * Sorteringar för en tabell med en kolumn index är nu i batch-läge.
  * Fönsterhantering mängder fungerar nu i batchläge, till exempel TSQL FÖRDRÖJNING/LEAD instruktioner.
  * Frågor om kolumnen Store tabeller med flera distinct-satser fungerar i Batch-läge.
  * Frågor som körs under DOP = 1 eller med en seriell plan också köras i Batch-läge.
* Senast, kardinalitet beräkning av förbättringar som faktiskt kommer med kompatibilitetsnivå 120, men för de körs på en lägre kompatibilitetsnivå (d.v.s. 100 eller 110), flytta kompatibilitet nivå 130 kommer också att få dessa förbättringar, och dessa kan också dra frågeprestandan för dina program.

## <a name="practicing-compatibility-level-130"></a>Öva kompatibilitetsnivå 130
Första ska få några tabeller, index och slumpmässiga data som skapats för att öva några av de nya funktionerna. Exempel för TSQL-skript kan köras under SQL Server 2016 eller under Azure SQL Database. Men när du skapar en Azure SQL database, kontrollera att du väljer minst en P2 databas eftersom du måste ha minst ett antal kärnor tillåta flertrådsteknik och därför utnyttja dessa funktioner.

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Nu ska vi ta en titt att vissa funktioner för bearbetning av frågan kommer med kompatibilitetsnivå 130.

## <a name="parallel-insert"></a>Parallell INSERT
Köra TSQL instruktionerna nedan kör INSERT-åtgärden under kompatibilitetsnivå 120 och 130, som utför respektive INSERT-åtgärden i en enda trådad modell (120) och i ett flertrådat modellen (130).

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Du kan bestämma vilka kardinalitet beräkning av funktionen är i play genom att begära den faktiska frågeplanen tittar på dess grafisk representation eller dess XML-innehåll. Granskar planer sida-vid-sida i figur 1 tydligt ser vi att kolumnen Store Infoga körningen går från seriella i 120 till parallellt i 130. Observera också att ändringen av iterator-ikonen i 130 planen visar två parallella pilar illustrerar faktumet att nu iterator-körningen är verkligen parallellt. Om du har stora INSERT-åtgärder att utföra fungerar parallell körning, länkade till antalet kärnor som du har på tillgänglig för databasen bättre; upp till 100 gånger snabbare beroende din situation!

*Bild 1: INSERT-åtgärden ändras från seriella till parallell kompatibilitet med nivå 130.*

![Bild 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a>SERIELL Batch-läge
Flytta till kompatibilitetsnivå 130 vid bearbetning av datarader aktiveras på motsvarande sätt, batchbearbetning läge. Först finns batchåtgärder läge bara när du har ett columnstore-index på plats. Därefter en batch representerar vanligen ~ 900 rader, och använder en kod logik som är optimerade för flera kärnor CPU, högre dataflödet i minnet och direkt utnyttjar komprimerade data för kolumnen Arkiv när det är möjligt. Under dessa förhållanden SQL Server 2016 kan bearbeta ~ 900 rader samtidigt, i stället för 1 rad vid tidpunkt och följaktligen övergripande omkostnaden för åtgärden nu delas av hela batchen, vilket minskar den totala kostnaden per rad. Den här delade mängd operationer som kombineras med kolumnen store komprimering i princip minskar svarstiden ingår i en SELECT batch-läge. Du kan hitta mer information om kolumnen store och batchläge på [Columnstore-index guiden](https://msdn.microsoft.com/library/gg492088.aspx).

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Som visas nedan, genom att följa frågan planer sida vid sida på bild 2 kan vi används se att bearbetningsläget har ändrats med en kompatibilitetsnivå och följaktligen när du kör frågor i båda kompatibilitetsnivå helt, kan vi se att det mesta av bearbetningstiden som i radläge (86%) jämfört med batch-läge (14%), där 2 batchar har bearbetats. Öka dataset, fördelen ökar.

*Bild 2: Välj åtgärden ändringar seriella till batchläge med en kompatibilitetsnivå 130.*

![Bild 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a>Batch-läge på Sortera körning
Övergången från radläge (kompatibilitetsnivå 120) till batch-läge (kompatibilitetsnivå 130) liknar ovan, men tillämpas sortering, förbättrar prestanda på sorteringen av samma skäl.

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Synliga sida-vid-sida på figur 3, vi se att sorteringen i radläge innebär 81% av kostnaden, medan batch-läge innebär bara 19% av kostnaden (respektive 81% och 56% på sorteringen sig själv).

*Bild 3: Sorteringsåtgärden ändras från raden till batch-läge med en kompatibilitetsnivå 130.*

![Bild 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

Naturligtvis är innehålla exemplen endast rader, tiotusentals som ingenting när du tittar på data som är tillgängliga i de flesta SQL Servers dessa dagar. Projektet bara dessa mot miljoner rader i stället detta kan översätta i flera minuter för körning av undantas varje dag väntande arten av din arbetsbelastning.

## <a name="cardinality-estimation-ce-improvements"></a>Förbättringar av kardinalitet uppskattning (CE)
Introducerades i SQL Server 2014 någon databas som körs på en kompatibilitetsnivå 120 eller över kommer genom att utnyttja nya kardinalitet beräkning av funktioner. Beräkning av kardinalitet är i princip den logik som används för att avgöra hur SQLServer ska köra en fråga baserat på dess uppskattade kostnaden. Uppskattningen beräknas med hjälp av indata från statistik som är associerade med objekt som är inblandade i frågan. Praktiskt taget, på en hög nivå kardinalitet beräkning av funktioner är raden antal uppskattningar tillsammans med information om distribution av värden, distinkta värdet inventeringar, och dubbla räknas som finns i de tabeller och objekten som refereras i frågan. Hämta dessa uppskattningar fel och kan leda till onödiga disk i/o på grund av otillräckligt minne ger (d.v.s. TempDB spill) eller till ett urval av en seriell plan körning via en plan för parallell körning till mera. Slutsats, felaktig beräknar kan leda till en övergripande prestandaförsämring i frågan. På den andra sidan bättre uppskattningar, exaktare beräknar leder till att bättre frågan körningar!

Som tidigare nämnts, fråga optimeringar och beräknar är en komplex fråga, men om du vill veta mer om frågeplaner och kardinalitet exteriörbedömning du kan referera till dokumentet på [optimera din frågeplaner med SQL Server 2014 kardinalitet exteriörbedömning](https://msdn.microsoft.com/library/dn673537.aspx) för en mer grundlig genomgång.

## <a name="which-cardinality-estimation-do-you-currently-use"></a>Vilka kardinalitet beräkning av du för närvarande använder?
Att fastställa under vilka kardinalitet beräkning av dina frågor körs, vi bara använda frågan exemplen nedan. Observera att det här första exemplet ska köras under kompatibilitetsnivån 110, innebär användning av de gamla kardinalitet beräkning av funktionerna.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


När körningen har slutförts, klicka på länken XML och titta på egenskaperna för den första iterator enligt nedan. Observera egenskapsnamnet kallas CardinalityEstimationModelVersion som för närvarande inställd på 70. Det innebär inte att databasens kompatibilitetsnivå har angetts till SQL Server 7.0-version (den är inställd på 110 som visas i TSQL-uttryck ovanför), men värdet 70 bara representerar bakåtkompatibla kardinalitet beräkning av funktionerna eftersom SQL Server 7.0 som hade ingen större ändringar förrän SQL Server 2014 (som levereras med en kompatibilitetsnivå på 120).

*Bild 4: CardinalityEstimationModelVersion har angetts till 70 när du använder en kompatibilitetsnivån 110 eller nedan.*

![Bild 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

Du kan också ändra kompatibilitetsnivån till 130 och inaktivera användningen av funktionen kardinalitet uppskattning med hjälp av LEGACY_CARDINALITY_ESTIMATION inställt på ON med [ALTER OMFÅNG DATABASKONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx). Det här är exakt detsamma som att använda 110 från en kardinalitet beräkning av funktionen synsätt, när du använder den senaste frågebearbetning kompatibilitetsnivå. Då kan du dra nytta av nya funktioner kommer med den senaste kompatibilitetsnivån (d.v.s. batchläge) för frågebearbetning, men fortfarande är beroende av gamla kardinalitet beräkning av funktionerna om det behövs.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Bara flyttas till kompatibilitetsnivå 120 eller 130 kan de nya funktionerna för beräkning av kardinalitet. I sådana fall standard CardinalityEstimationModelVersion anges därför till 120 eller 130 som visas nedan.

```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Bild 5: CardinalityEstimationModelVersion har angetts till 130 när du använder en kompatibilitetsnivå 130.*

![Bild 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-the-cardinality-estimation-differences"></a>Bevittnat kardinalitet beräkning av skillnaderna
Nu ska vi kör lite mer komplex fråga som involverar en INNER JOIN med en WHERE-sats med vissa predikat och titta vi på raden antal uppskattning från den gamla kardinalitet beräkning av funktionen först.

```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Den här frågan körs effektivt returnerar 200,704 rader när raden uppskattning med funktionen gamla kardinalitet beräkning av anspråk 194,284 rader. Naturligtvis, som säger innan dessa rad antal resultat beror också hur ofta du körde tidigare exemplen som fyller tabellerna exempel upprepade gånger vid varje körning. Naturligtvis är predikat i frågan har också en inverkan på den faktiska uppskattningen utöver tabellform innehåll, data och hur dessa data faktiskt korrelera med varandra.

*Bild 6: Raden antal uppskattning är 194,284 eller 6 000 rader av från 200,704 rader som förväntat.*

![Bild 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

På samma sätt du vi nu köra samma fråga med de nya funktionerna för beräkning av kardinalitet.

```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Titta på den nedan, vi nu se att raden uppskattning är 202,877, eller mycket närmare och högre än den gamla kardinalitet uppskattning.

*Bild 7: Raden antal uppskattning är nu 202,877 i stället för 194,284.*

![Bild 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

I verkligheten kan resultatet är 200,704 rader (men alla beror på hur ofta du körde frågorna av föregående exempel, men viktigare, eftersom TSQL använder instruktionen RAND(), de faktiska värden som returneras kan variera från att köras som en till nästa). Därför i det här exemplet är nya kardinalitet uppskattning bättre på att uppskatta hur många rader eftersom 202,877 är mycket närmare 200,704, än 194,284! Senaste om du ändrar WHERE-satsen predikat till likheten (snarare än ”>” för instans), detta kan vara en beräknar mellan gammalt och nya kardinalitet funktionen ännu mer olika beroende på hur många matchningar du kan hämta.

Naturligtvis är i det här fallet representerar som ~ 6000 rader ut från det faktiska antalet inte en stor mängd information i vissa situationer. TRANSPONERA detta miljoner rader över flera tabeller och mer komplexa frågor och ibland uppskattning kan vara inaktiverat av miljoner rader och därmed risken för fel åtgärdsplan plockning upp eller begär inte tillräckligt med minne ger leder till TempDB spill och därför flera i/o är nu mycket högre.

Om du har möjlighet praxis denna jämförelse med vanliga frågor och datauppsättningar, och se själv av hur mycket några av de gamla och nya beräkningarna påverkas, medan vissa bara kan bli mer ut från de i verkligheten använder eller några andra bara bara närmare till den faktiska raden räknar faktiskt returnerade i resultatmängder. Alla beror på formen av dina frågor, Azure SQL database-egenskaper, natur och storleken på dina datauppsättningar och tillgänglig om dem statistik. Om du just har skapat din Azure SQL Database-instans måste Frågeoptimeringen skapa sin kunskap från grunden i stället för att återanvända statistik av de föregående frågan körs. Beräkningarna är mycket kontextuella och nästan specifika för varje server- och situation. Det är en viktig aspekt att tänka på!

## <a name="some-considerations-to-take-into-account"></a>Vissa aspekter att beakta
De flesta arbetsbelastningar lönar sig att kompatibilitetsnivån 130, innan du anta kompatibilitetsnivån för produktionsmiljön, men du har i princip 3 alternativ:

1. Du flyttar till kompatibilitetsnivå 130 och se hur saker utföra. Om du märker att vissa regressioner du bara bara ange kompatibilitet tillbaka till den ursprungliga nivån eller behålla 130 och endast omvänd kardinalitet uppskattning tillbaka till bakåtkompatibelt läge (som beskrivs ovan, detta fristående kan åtgärda problemet).
2. Testa din befintliga program under liknande produktion belastning, finjustera och validera prestanda innan du fortsätter till produktionen. Vid problem, samma som ovan, du kan alltid gå tillbaka till den ursprungliga kompatibilitetsnivån eller helt enkelt Återför kardinalitet uppskattning till bakåtkompatibelt läge.
3. Sista alternativet, och det senaste sättet att åtgärda dessa frågor, är att använda Query Store. Det är dagens rekommenderade alternativet! För att hjälpa analys av dina frågor under kompatibilitet nivå 120 eller nedan jämfört med 130, kan du gärna nog att använda Query Store. Query Store är tillgänglig med den senaste versionen av Azure SQL Database V12 och den har utformats för att hjälpa dig med felsökning av prestanda i frågan. Query Store kan liknas vid en svarta lådan för data för din databas för att samla in och presentera historisk information om alla frågor. Detta underlättar avsevärt dataforensik prestanda genom att minska tiden för att diagnostisera och lösa problem. Du hittar mer information i [Query Store: en svarta lådan för data för databasen](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

AT på hög nivå, om du redan har en uppsättning databaser som körs i kompatibilitetsnivå 120 eller under och planerar att flytta en del av dem till 130 eller eftersom din arbetsbelastning automatiskt etablera nya databaser som kommer att vara snart anges som standard till 130, Överväg följande:

* Aktivera Query Store innan du ändrar till nya kompatibilitetsnivån i produktion. Du kan referera till [ändra databasens kompatibilitetsläge och använda Frågearkivet](https://msdn.microsoft.com/library/bb895281.aspx) för mer information.
* Sedan testa alla kritiska arbetsbelastningar som använder representativt data och frågor av produktions-liknande miljö och jämföra prestanda erfarenhet och som rapporterades av Query Store. Om det uppstår några regressioner identifiera regressed frågor med Query Store och använda planen tvinga alternativet från Query Store (aka planera fästning). I sådana fall du slutgiltigt förblir med en kompatibilitetsnivå 130 och använda tidigare frågeplanen föreslaget av Query Store.
* Om du vill utnyttja nya funktioner och funktioner i Azure SQL Database (som kör SQL Server 2016), men är känsliga för ändringar av kompatibilitetsnivån 130, i nödfall, kan du tvinga kompatibilitetsnivån tillbaka till den nivå som passar din arbetsbelastning med hjälp av ALTER DATABASE-instruktionen. Men först måste vara medveten om att Query Store planen fästning alternativet är det bästa alternativet eftersom du inte använder 130 i princip används funktionalitetsnivån en äldre version av SQL Server.
* Om du har flera program som sträcker sig över flera databaser kan vara det nödvändigt att uppdatera etablering logiken för dina databaser för att säkerställa en konsekvent kompatibilitetsnivå över alla databaser. de gamla och nya etablerats. Din arbetsbelastning programprestanda kan vara känslig till att vissa databaser körs vid olika kompatibilitetsnivåer och därför kompatibilitet nivå konsekvent på alla databaser kan vara nödvändiga för att erbjuda kunderna samma upplevelse alla i alla. Observera att det inte är uppdrag, det beror på hur ditt program påverkas av kompatibilitetsnivå.
* Senast, om kardinalitet uppskattning och precis som ändrar kompatibilitetsnivå innan du fortsätter i produktion, rekommenderas att testa din produktion arbetsbelastning på nya villkor för att avgöra om ditt program fördelar från kardinalitet uppskattning förbättringar.

## <a name="conclusion"></a>Slutsats
Med Azure SQL Database för att dra nytta av alla SQL Server 2016 förbättringar kan du förbättra din fråga körningar tydligt. Precis som-är! Naturligtvis som en ny funktion, en ordentlig utvärdering måste du göra för att fastställa exakta villkor under vilka arbetsbelastningen databasen fungerar bäst. Erfarenheterna visar att de flesta arbetsbelastning förväntas minst köras transparent kompatibilitetsnivå 130, samtidigt utnyttja nya frågebearbetning funktioner och ny kardinalitet uppskattning. Som säger normalt är alltid är vissa undantag och göra korrekt betalning fordringar är en viktig bedömning att avgöra hur mycket du kan dra nytta av dessa förbättringar. Och igen, Frågearkivet kan vara av en stor hjälp om du gör detta verk!

Då SQL Azure utvecklas, du kan förvänta dig en kompatibilitetsnivå 140 i framtiden. När tid är lämpligt, startas vi pratar om vad den här framtida kompatibilitetsnivå 140 kommer att få, precis som vi kort beskrivs här vilka kompatibilitetsnivå 130 gör i dag.

Nu ska vi inte glömma, från juni 2016 är Azure SQL Database ändras kompatibilitetsnivån standard från 120 till 130 för nyskapade databaser. Tänk!

## <a name="references"></a>Referenser
* [Vad är nytt i databasmotor](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [Blogg: Query Store: en svarta lådan för data för databasen, av Borko Novakovic, juni 8 2016](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [Ändra DATABASENS kompatibilitetsnivån (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)
* [ALTER DATABASE OMFÅNG KONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx)
* [Kompatibilitetsnivån 130 för Azure SQL Database V12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [Optimera din fråga planer med SQLServer 2014 kardinalitet exteriörbedömning](https://msdn.microsoft.com/library/dn673537.aspx)
* [Guide för Columnstore-index](https://msdn.microsoft.com/library/gg492088.aspx)
* [Blogg: Förbättrad prestanda för frågor med kompatibilitetsnivå 130 i Azure SQL-databas med Alain Lissoir maj 6 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
