---
title: "aaaDatabase kompatibilitetsnivå 130 - Azure SQL Database | Microsoft Docs"
description: "I den här artikeln vi utforska hello fördelarna med att köra Azure SQL Database på kompatibilitetsnivå 130 och utnyttja hello fördelarna med hello nya frågeoptimeraren och fråga processorfunktioner. Vi tar också upp hello möjliga sidoeffekter på hello frågeprestanda för hello befintliga SQL-program."
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
ms.openlocfilehash: 25693c5f7b01405b7073fa7d4cc2833fbe125e2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Förbättrad prestanda för frågor med nivå 130 i Azure SQL Database
Azure SQL Database körs transparent hundratals tusentals databaser på många olika kompatibilitetsnivåer bevara och garanterar hello bakåtkompatibilitet toohello motsvarande version av Microsoft SQL Server för alla kunder!

I den här artikeln vi utforska hello fördelarna med kör din Azure SQL-Databse på kompatibilitetsnivå 130 och utnyttja hello fördelarna med hello nya frågeoptimeraren och fråga processorfunktioner. Vi tar också upp hello möjliga sidoeffekter på hello frågeprestanda för hello befintliga SQL-program.

Som en påminnelse om historik hello justeringen för SQL-versioner toodefault kompatibilitetsnivåer är följande:

* 100: i SQL Server 2008 och Azure SQL Database V11.
* 110: i SQL Server 2012 och Azure SQL Database V11.
* 120: i SQL Server 2014 och Azure SQL Database V12.
* 130: i SQL Server 2016 och Azure SQL-databasen (V12).

> [!IMPORTANT]
> Från och med **mid juni 2016**, i Azure SQL Database hello standard kompatibilitetsnivå blir 130 i stället för 120 för **nyskapad** databaser.
> 
> Databaser som skapats före mitten av juni 2016 kommer *inte* påverkas och kommer bibehålla sina aktuella kompatibilitetsnivån (100, 110 eller 120). Databaser som migrerats från Azure SQL Database version V11 tooV12 har en kompatibilitetsnivå 100 eller 110. 
> 

## <a name="about-compatibility-level-130"></a>Om kompatibilitetsnivåer 130
Om du vill tooknow hello aktuella kompatibilitetsnivån för databasen du först köra hello följande Transact-SQL-instruktion.

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Före den här ändringen sker toolevel 130 för **nyligen** skapat databaser, vi gå igenom den här ändringen handlar om till exempel grundläggande frågan och se hur vem som helst kan dra nytta av den.

Frågebearbetning i relationsdatabaser kan vara väldigt komplexa och kan leda till toolots datorn vetenskap, räkning toounderstand hello inbyggd designalternativen och beteenden. I det här dokumentet har hello innehåll avsiktligt förenklad tooensure att vem som helst med vissa minsta teknisk bakgrund förstår hello effekten av hello kompatibilitet förändring och ta reda på hur det kan göra program.

Vi har en titt på vilka hello kompatibilitetsnivå 130 ger när hello tabellen.  Du hittar mer information i [ändra DATABASENS kompatibilitetsnivån (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), men här är en kort sammanfattning:

* hello Insert-åtgärden för en Insert select-instruktion kan vara flertrådade eller en parallell plan medan innan den här åtgärden var Enkeltrådig.
* Optimerad tabell och tabellen variabler frågor kan nu använda parallella planer minne medan innan den här åtgärden även var Enkeltrådig.
* Statistik för Minnesoptimerade tabellen kan nu samlas in och kan uppdateras automatiskt. Se [vad är nytt i Database Engine: Minnesintern OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) för mer information.
* Batch-läge v/s radläge ändras med kolumnen index
  * Sorteringar för en tabell med en kolumn index är nu i batch-läge.
  * Fönsterhantering mängder fungerar nu i batchläge, till exempel TSQL FÖRDRÖJNING/LEAD instruktioner.
  * Frågor om kolumnen Store tabeller med flera distinct-satser fungerar i Batch-läge.
  * Frågor som körs under DOP = 1 eller med en seriell plan också köras i Batch-läge.
* Senast, kardinalitet beräkning av förbättringar som faktiskt kommer med kompatibilitetsnivå 120, men för de körs på en lägre kompatibilitet (d.v.s. 100 eller 110) hello flytta toocompatibility nivå 130 kommer också att få dessa förbättringar och de kan också Dra hello frågeprestandan för dina program.

## <a name="practicing-compatibility-level-130"></a>Öva kompatibilitetsnivå 130
Första ska få några tabeller, index och slumpmässiga data som har skapats toopractice några av de nya funktionerna. exempel på hello TSQL-skript kan köras under SQL Server 2016 eller under Azure SQL Database. Men när du skapar en Azure SQL database, kontrollera att du väljer på hello minsta en P2 databasen eftersom du måste ha minst ett antal kärnor tooallow flertrådsteknik och därför utnyttja dessa funktioner.

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- hello second one (only available on Premium databases)

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


Nu har vi en titt toosome hello bearbetning av frågan funktioner kommer med kompatibilitetsnivå 130.

## <a name="parallel-insert"></a>Parallell INSERT
Köra hello TSQL instruktionerna nedan kör hello INSERT-åtgärden under kompatibilitetsnivå 120 och 130, som utför respektive hello INSERT-åtgärden i en enda trådad modell (120) och i ett flertrådat modellen (130).

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- hello INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- hello INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Du kan bestämma vilka kardinalitet beräkning av funktionen är i play genom att begära hello faktiska hello frågeplan, titta på dess grafisk representation eller dess XML-innehåll. Granskar hello planer sida-vid-sida i figur 1 tydligt ser vi att hello kolumnen Store Infoga körning går från seriella i 120 tooparallel i 130. Observera också hello ändringen av hello iterator ikon i hello 130 plan för två parallella pilar illustrerar hello fakta som nu hello iterator-körningen är verkligen parallellt. Om du har stora INSERT operations toocomplete fungerar hello parallell körning, länkade toohello antal kärnor som du har på tillgänglig för hello databasen bättre; beroende din situation in tooa 100 gånger snabbare!

*Bild 1: Infoga åtgärd ändringarna från seriella tooparallel med kompatibilitetsnivå 130.*

![Bild 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a>SERIELL Batch-läge
Flytta toocompatibility nivå 130 vid bearbetning av datarader aktiveras på motsvarande sätt, batchbearbetning läge. Först finns batchåtgärder läge bara när du har ett columnstore-index på plats. Därefter en batch vanligtvis representerar ~ 900 rader och använder en kod logik som är optimerade för flera kärnor CPU, högre dataflödet i minnet och direkt använder hello komprimerade data för hello kolumnen Store när det är möjligt. Under dessa förhållanden SQL Server 2016 kan bearbeta ~ 900 rader samtidigt, i stället för 1 rad hello tidpunkt och följaktligen hello övergripande omkostnaden för hello åtgärden nu delas av hello hela batchen, minska hello övergripande kostnader per rad. Den här delade mängd operationer som tillsammans med hello kolumnen store komprimering i princip minskar hello latens ingår i en SELECT batch-läge. Du kan hitta mer information om hello kolumnen store och batchläge på [Columnstore-index guiden](https://msdn.microsoft.com/library/gg492088.aspx).

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Som visas nedan, ser genom att följa hello frågan planer sida-vid-sida på bild 2, vi att hello bearbetningsläget har ändrats med hello kompatibilitetsnivå och följaktligen när du kör hello frågor i båda kompatibilitetsnivå helt, kan vi se som de flesta av hello bearbetningstid används i rad läget (86%) jämfört med toohello batch-läge (14%), där 2 batchar har bearbetats. Öka hello dataset, hello förmånen ökar.

*Bild 2: Välj åtgärden ändringar från seriella toobatch läge med en kompatibilitetsnivå 130.*

![Bild 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a>Batch-läge på Sortera körning
Liknande toohello ovan, men tillämpade tooa sorteringsåtgärden hello övergången från radläge läge (kompatibilitetsnivå 120) toobatch (kompatibilitetsnivå 130) förbättrar hello prestanda på hello sorteringsåtgärden för hello samma orsaker.

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Synliga sida-vid-sida på figur 3, vi se att hello sorteringsåtgärden i radläge representerar 81% av hello kostnad, medan hello batchläge representerar bara 19% av hello kostnaden (respektive 81% och 56% på hello sortera sig själv).

*Bild 3: Sorteringsåtgärden ändras från toobatch radläge med kompatibilitetsnivå 130.*

![Bild 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

Naturligtvis är innehålla exemplen endast rader, tiotusentals som ingenting när du tittar på hello data är tillgängliga i de flesta SQL Servers dessa dagar. Projektet bara dessa mot miljoner rader i stället detta kan översätta i flera minuter för körning av undantas varje dag väntande hello uppbyggnad din arbetsbelastning.

## <a name="cardinality-estimation-ce-improvements"></a>Förbättringar av kardinalitet uppskattning (CE)
Introducerades i SQL Server 2014, en databas som körs på en kompatibilitetsnivå 120 eller över kommer att använda hello nya kardinalitet beräkning av funktioner. Beräkning av kardinalitet är i princip hello logik används toodetermine hur SQLServer ska köra en fråga baserat på dess uppskattade kostnaden. beräkning av hello beräknas med hjälp av indata från statistik som är associerade med objekt som är inblandade i frågan. Praktiskt taget, på en hög nivå kardinalitet beräkning av funktioner är raden antal uppskattningar tillsammans med information om hello distribution av hello värden, distinkta värdet inventeringar, och dubbla räknas som ingår i hello tabeller och objekt som refereras till i hello frågan. Hämta dessa uppskattningar fel kan leda till att toounnecessary disk i/o på grund av tooinsufficient minne ger (d.v.s. TempDB spill) eller tooa val av en seriell plan körning via en parallell planerar körning och tooname några. Slutsats, felaktig beräknar leda tooan övergripande prestanda försämras av hello Frågekörningen. Hej på andra sidan, bättre uppskattningar, exaktare beräknar, leads toobetter frågan körningar!

Som tidigare nämnts, fråga optimeringar och beräknar är en komplex fråga, men om du vill toolearn mer om frågeplaner och kardinalitet exteriörbedömning, kan du läsa toohello dokumentet på [optimera din frågeplaner med hello SQL Server 2014 Kardinalitet exteriörbedömning](https://msdn.microsoft.com/library/dn673537.aspx) för en mer grundlig genomgång.

## <a name="which-cardinality-estimation-do-you-currently-use"></a>Vilka kardinalitet beräkning av du för närvarande använder?
toodetermine under vilka kardinalitet beräkning av dina frågor körs, vi bara Använd hello frågan exempel nedan. Observera att det här första exemplet ska köras under kompatibilitetsnivån 110, innebär hello användning av hello gamla kardinalitet beräkning av funktioner.

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


När körningen har slutförts klickar du på hello XML-länk och titta på hello egenskaper för hello första iterator enligt nedan. Observera hello egenskapsnamn som kallas CardinalityEstimationModelVersion som för närvarande inställd på 70. Det innebär inte att hello databasens kompatibilitetsnivå är toohello SQL Server 7.0 version (den är inställd på 110 som visas i hello TSQL-uttryck ovanför), men hello värdet 70 representerar bara hello äldre kardinalitet beräkning av funktionerna eftersom SQL Server 7.0, som hade ingen större ändringar förrän SQL Server 2014 (som levereras med en kompatibilitetsnivå på 120).

*Bild 4: hello CardinalityEstimationModelVersion anges too70 när du använder en kompatibilitetsnivån 110 eller nedan.*

![Bild 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

Du kan också du kan ändra hello kompatibilitet nivå too130 och inaktiverar hello användning av hello nya kardinalitet beräkning av funktionen med hjälp av hello LEGACY_CARDINALITY_ESTIMATION ange tooON med [ALTER OMFÅNG DATABASKONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx). Detta kommer att exakt hello samma som med 110 från en kardinalitet beräkning av funktionen synsätt, när du använder hello senaste frågebearbetning kompatibilitetsnivå. Då kan du dra nytta av hello nya frågebearbetning funktioner kommer med hello senaste kompatibilitetsnivå (d.v.s. batchläge), men fortfarande är beroende av hello gamla kardinalitet beräkning av funktionerna om det behövs.

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


Bara flytta toohello kompatibilitetsnivå 120 eller 130 kan hello nya kardinalitet beräkning av funktioner. I sådana fall anges hello standard CardinalityEstimationModelVersion därför too120 eller 130 som visas nedan.

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


*Bild 5: hello CardinalityEstimationModelVersion anges too130 när du använder en kompatibilitetsnivå 130.*

![Bild 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-hello-cardinality-estimation-differences"></a>Bevittnat hello kardinalitet uppskattning skillnader
Nu ska vi kör lite mer komplex fråga som involverar en INNER JOIN med en WHERE-sats med vissa predikat och till hur känna hello rad antal uppskattning från hello gamla kardinalitet beräkning av funktionen först.

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


Den här frågan körs effektivt returnerar 200,704 rader medan hello raden uppskattning med hello gamla kardinalitet uppskattning funktionalitet anspråk 194,284 rader. Naturligtvis, som säger innan dessa rad antal resultat beror också hur ofta du körde hello tidigare prov som fyller hello Exempeltabeller upprepade gånger vid varje körning. Naturligtvis, har också hello predikat i frågan påverkar hello faktiska uppskattning utöver hello tabellform datainnehållet och hur dessa data faktiskt korrelera med varandra.

*Bild 6: hello rad antal uppskattning är 194,284 eller 6 000 rader ut från hello 200,704 rader som förväntat.*

![Bild 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

I hello samma sätt kan vi nu köra hello samma fråga med hello nya kardinalitet beräkning av funktioner.

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


Titta på hello nedan Se vi nu att hello raden uppskattning är 202,877, eller mycket närmare och högre än hello gamla kardinalitet uppskattning.

*Bild 7: hello rad antal uppskattning är nu 202,877 i stället för 194,284.*

![Bild 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

I själva verket hello resultatmängden är 200,704 rader (men alla beror på hur ofta du körde hello frågor för hello föregående exempel, men viktigare, eftersom hello TSQL använder hello RAND() instruktionen, hello faktiska värden som returneras kan variera från en kör toohello nästa). Därför i det här exemplet är hello nya kardinalitet uppskattning bättre på att uppskatta hello antalet rader eftersom 202,877 är mycket närmare too200, 704, än 194,284! Senaste om du ändrar hello WHERE-satsen predikat tooequality (snarare än ”>” för instans), det gick att hello beräknar mellan hello gamla och nya kardinalitet funktionen ännu mer olika, beroende på hur många matchningar som du kan hämta.

Naturligtvis är i det här fallet representerar som ~ 6000 rader ut från det faktiska antalet inte en stor mängd information i vissa situationer. Nu kan införliva denna toomillions rader över flera tabeller och mer komplexa frågor och ibland hello uppskattning kan vara inaktiverat av miljoner rader och därför hello risken för plockning upp hello fel åtgärdsplan eller begär inte tillräckligt med minne ger inledande tooTempDB spill och därför flera i/o är mycket högre.

Om du har möjlighet hello öva denna jämförelse med vanliga frågor och datauppsättningar och se själv med hur mycket vissa hello gamla och nya beräknar påverkas, medan vissa bara kan bli mer ut från hello verkligheten eller några andra bara bara närmare radantalet toohello faktiska faktiskt returneras i hello resultatmängder. Alla beror på hello form av dina frågor, hello Azure SQL database egenskaper, hello hello storlek och dina datauppsättningar och hello statistik om dem. Om du just har skapat din Azure SQL Database-instans, hello fråga optimering har toobuild körs sin kunskap från grunden i stället för att återanvända statistik av hello föregående frågan. Hello uppskattningar är så mycket kontextuella och nästan specifika tooevery server- och situation. Det är en viktig aspekt tookeep i åtanke!

## <a name="some-considerations-tootake-into-account"></a>Vissa överväganden tootake hänsyn
De flesta arbetsbelastningar skulle utnyttja hello kompatibilitetsnivå 130, innan du använder hello kompatibilitetsnivån för produktionsmiljön, men du har i princip 3 alternativ:

1. Du flytta toocompatibility nivå 130 och se hur saker utföra. Om du upptäcker några regressioner bara bara ange hello kompatibilitet nivån tillbaka tooits ursprungliga nivå eller behålla 130 och endast återföra hello kardinalitet uppskattning tillbaka toohello bakåtkompatibelt läge (som beskrivs ovan, detta fristående kunde adress hello problemet).
2. Testa din befintliga program under liknande produktion belastning, finjustera och validera hello prestanda innan pågående tooproduction. Vid problem, samma som ovan, du kan alltid gå tillbaka toohello ursprungliga kompatibilitetsnivå eller helt enkelt omvänd hello kardinalitet uppskattning tillbaka toohello bakåtkompatibelt läge.
3. Som en sista alternativet och hello senaste sätt tooaddress dessa frågor är tooleverage hello Query Store. Det är dagens rekommenderade alternativet! tooassist hello analys av dina frågor under kompatibilitet nivå 120 eller nedan jämfört med 130, kan du gärna tillräckligt med toouse Query Store. Query Store är tillgänglig med hello senaste versionen av Azure SQL Database V12 och den är utformad för toohelp du felsökningen av prestanda i frågan. Se hello Query Store som ett svarta lådan för data för din databas för att samla in och presentera historisk information om alla frågor. Detta förenklar dataforensik prestanda genom att minska hello tid toodiagnose avsevärt och lösa problem. Du hittar mer information i [Query Store: en svarta lådan för data för databasen](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

På hög nivå om du redan har en uppsättning databaser som körs i kompatibilitetsnivå 120 eller under och planera toomove hello några av dem too130, eller att din arbetsbelastning automatiskt etablera nya databaser som ska snart vara som standard too130, Överväg hello följande:

* Aktivera Query Store innan du ändrar toohello nya kompatibilitetsnivå i produktion. Du kan se för[ändra hello databasens kompatibilitetsläge och använda hello Query Store](https://msdn.microsoft.com/library/bb895281.aspx) för mer information.
* Sedan testa alla kritiska arbetsbelastningar som använder representativt data och frågor av produktions-liknande miljö och jämföra hello prestanda erfarenhet och som rapporterades av Query Store. Om det uppstår några regressioner, kan du identifiera hello regressed frågor med hello Query Store och använda hello plan för att tvinga alternativet från Query Store (aka planera fästning). I så fall kan du slutgiltigt förblir med hello kompatibilitetsnivå 130 och använda hello tidigare frågeplan som föreslagna av hello Query Store.
* Om du vill tooleverage nya funktioner och funktioner i Azure SQL Database (som kör SQL Server 2016), men är känsliga toochanges som hello kompatibilitetsnivå 130, som en sista utväg kan du tvinga hello kompatibilitetsnivå tillbaka toohello nivå som passar din arbetsbelastning med hjälp av ALTER DATABASE-instruktionen. Men Tänk först hello Query Store planen fästning alternativet är det bästa alternativet eftersom du inte använder 130 i princip används funktionalitetsnivån hello en äldre version av SQL Server.
* Om du har flera program som sträcker sig över flera databaser kan det vara nödvändigt tooupdate hello etablering logiken för dina databaser tooensure en konsekvent kompatibilitetsnivå över alla databaser. de gamla och nya etablerats. Din arbetsbelastning programprestanda kan vara känslig toohello faktum att vissa databaser körs vid olika kompatibilitetsnivåer och därför kompatibilitet nivå konsekvent på alla databaser kan krävas för tooprovide hello samma få tooyour kunder alla över hello-kort. Observera att det inte är uppdrag, det beror på hur ditt program påverkas av hello kompatibilitetsnivå.
* Senast, om hello kardinalitet uppskattning och precis som ändrar hello kompatibilitetsnivå innan du fortsätter i produktion, är det rekommenderade tootest produktion arbetsbelastningen under hello nya villkor toodetermine om tillämpningsprogrammet fördelar från hello kardinalitet uppskattning förbättringar.

## <a name="conclusion"></a>Slutsats
Med Azure SQL Database kan toobenefit från alla SQL Server 2016 förbättringar tydligt förbättra din fråga körningar. Precis som-är! Naturligtvis som en ny funktion, måste en ordentlig utvärdering göras toodetermine hello exakt villkor under vilka arbetsbelastningen databasen fungerar hello bäst. Erfarenheterna visar att de flesta arbetsbelastningen är förväntade tooat minst köras transparent kompatibilitetsnivå 130, samtidigt utnyttja nya frågebearbetning funktioner och ny kardinalitet uppskattning. Som säger normalt är alltid är vissa undantag och göra korrekt betalning fordringar är ett viktigt assessment toodetermine hur mycket du kan dra nytta av dessa förbättringar. Och igen, hello Query Store kan vara av en stor hjälp om du gör detta verk!

Då SQL Azure utvecklas, du kan förvänta dig en kompatibilitetsnivå 140 i hello framtida. När tid är lämpligt, startas vi pratar om vad den här framtida kompatibilitetsnivå 140 kommer att få, precis som vi kort beskrivs här vilka kompatibilitetsnivå 130 gör i dag.

Nu ska vi inte glömma, från juni 2016 är Azure SQL Database ändras hello standard kompatibilitetsnivå från 120 too130 för nyskapade databaser. Tänk!

## <a name="references"></a>Referenser
* [Vad är nytt i databasmotor](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [Blogg: Query Store: en svarta lådan för data för databasen, av Borko Novakovic, juni 8 2016](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [Ändra DATABASENS kompatibilitetsnivån (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)
* [ALTER DATABASE OMFÅNG KONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx)
* [Kompatibilitetsnivån 130 för Azure SQL Database V12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [Optimera din fråga planer med hello SQL Server 2014 kardinalitet exteriörbedömning](https://msdn.microsoft.com/library/dn673537.aspx)
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
