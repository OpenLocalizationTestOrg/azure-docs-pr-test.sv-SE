---
title: aaaDistributing tabeller i SQL Data Warehouse | Microsoft Docs
description: "Komma igång med att distribuera tabeller i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 65093eeaeb00fef85aaa6070da2c976fed3f4bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a>Distribuera tabeller i SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Översikt över][Overview]
> * [Datatyper][Data Types]
> * [Distribuera][Distribute]
> * [Index][Index]
> * [Partition][Partition]
> * [Statistik][Statistics]
> * [Tillfällig][Temporary]
>
>

SQL Data Warehouse är ett distribuerat MPP-databassystem.  Genom att dela upp data- och bearbetningsfunktioner på flera noder kan SQL Data Warehouse erbjuda fantastisk skalbarhet – långt över den i ett enskilt system.  Bestämmer hur toodistribute dina data i ditt SQL Data Warehouse är en av de viktigaste hello faktorer tooachieving optimala prestanda.   hello viktiga toooptimal prestanda minimera dataförflyttning och i sin tur hello viktiga toominimizing dataflyttning väljer hello rätt distributionsstrategi.

## <a name="understanding-data-movement"></a>Förstå dataflyttning
Hello data från varje tabell fördelas på flera underliggande databaser i en MPP-system.  hello mest optimerade frågor på ett MPP-system kan bara skickas via tooexecute på hello enskilda distribuerade databaser utan interaktion mellan hello andra databaser.  Anta exempelvis att du har en databas med försäljningsdata som innehåller två tabeller, försäljning och kunder.  Om du har en fråga som måste toojoin din försäljning tooyour kundtabellen och du dividerar både försäljnings- och kunden tabeller upp med kundnummer, lägga till varje kund i en separat databas, kan frågor som förenar försäljning och kunder lösas inom varje databasen med ingen kunskap om hello andra databaser.  Däremot om du har delat din försäljningsdata genom ordningsnummer och din kundinformation på kundnummer sedan alla angivna databasen inte hello motsvarande data för varje kund och därmed om du vill ha toojoin kunddata försäljningsdata tooyour behöver du tooget hello data för varje kund från hello andra databaser.  I den här andra exemplet behöver dataflyttning toooccur toomove hello data toohello försäljning kundinformation, så att hello två tabeller kan vara ansluten.  

Dataflyttning inte alltid dåliga konsekvenser, ibland är det nödvändigt toosolve en fråga.  Men när den här extra steg kan undvikas, naturligt frågan körs snabbare.  Dataflyttning uppstår vanligen när tabellerna eller aggregeringar utförs.  Du behöver ofta toodo båda, så när du toooptimize för ett scenario som en koppling du fortfarande behöver data movement toohelp du lösa för hello andra scenarier, t.ex. en aggregering.  hello tips är att räkna ut som är mindre arbete.  I de flesta fall är distribuerar stora faktatabeller i en kolumn som är ofta kopplade hello effektivaste metoden för att minska hello de flesta flytt av data.  Distribuera data på kopplingskolumner är en mycket mer vanliga tooreduce för metoden dataflyttning än distribuerar data på kolumner som ingår i en samling.

## <a name="select-distribution-method"></a>Välj distributionsmetod
Hello bakgrunden delar dina data i 60 databaser i SQL Data Warehouse.  Varje enskild databas är refererad tooas en **distribution**.  När data läses in i varje tabell, SQL Data Warehouse har tooknow hur toodivide data mellan dessa 60 distributioner.  

hello distributionsmetod definieras på hello tabell nivå och det finns två alternativ:

1. **Resursallokering** som distribuera data jämnt men slumpmässigt.
2. **Hash-distribuerade** som distribuerar data baserat på hash-värden från en enda kolumn

Som standard när du inte definierar en data-distributionsmetod tabellen kommer att distribueras med hjälp av hello **resursallokering** distributionsmetod.  När du blir mer avancerade i implementeringen av du kommer också tooconsider med **hash distribuerade** tabeller toominimize dataflyttning som i sin tur optimerar prestanda för frågor.

### <a name="round-robin-tables"></a>Resursallokering tabeller
Använda hello resursallokering metod för att distribuera data är mycket hur det låter.  Eftersom dina data har lästs in, skickas bara toohello nästa distribution varje rad.  Den här metoden för att distribuera hello data ska alltid slumpmässigt distribuera hello data mycket jämnt över alla hello-distributioner.  Det är ingen sortering klart under hello round robin process som placerar dina data.  En distributionsplats för resursallokering kallas ibland ett slumpmässigt hash därför.  Med en distribuerad tabell resursallokering finns inga måste toounderstand hello data.  Därför vara resursallokering tabeller en bra inläsning mål.

Som standard om du väljer Ingen distributionsmetod används hello resursallokering distributionsmetod.  Medan resursallokering tabeller är enkla toouse eftersom data är slumpmässigt fördelad över hello system innebär det att hello system inte kan garantera vilken distribution är varje rad på.  Som ett resultat, ibland hello-system måste tooinvoke ordna dina data i en data movement åtgärden toobetter innan den kan lösa en fråga.  Den här extra steg kan sakta ned dina frågor.

Överväg att använda resursallokering (Round robin) för en tabell i hello följande scenarier:

* När igång som en enkel startpunkt
* Om det finns ingen uppenbara anslutande nyckel
* Om det inte är bra kandidat kolumn för hash-distribuerar hello tabell
* Om hello dela tabellen inte en gemensam join-nyckel med andra tabeller
* Om hello koppling är mindre viktig än andra kopplingar i hello frågan
* När hello tabell är en tillfällig mellanlagring tabell

Båda dessa exempel skapar en resursallokering tabell:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> Medan resursallokering hello tabell standardtypen är explicit i din DDL betraktas som bästa praxis så att din tabellayout hello avsikt Rensa tooothers.
>
>

### <a name="hash-distributed-tables"></a>Hash-distribuerade tabeller
Med hjälp av en **Hash distribuerade** algoritmen toodistribute tabeller kan förbättra prestanda för många scenarier genom att minska dataflyttning frågan för närvarande.  Hash distribuerade tabeller är tabeller som delas mellan hello distribuerade databaser med hjälp av en hash-algoritm på en enda kolumn som du väljer.  hello distribution kolumnen är vad avgör hur hello data fördelas på dina distribuerade databaser.  hello hash-funktionen använder hello distribution kolumnen tooassign rader toodistributions.  Hej hash-algoritm och resulterande distribution är deterministisk.  Hello som är identiskt med hello samma datatyp kommer alltid har toohello samma distribution.    

Det här exemplet skapar en tabell som distribueras på-id:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Välj distributionsplatser kolumn
När du väljer för**hash distribuera** en tabell, behöver du tooselect en enkel distribution-kolumn.  När du väljer en kolumn för distribution, finns det tre viktiga faktorer tooconsider.  

Markera en kolumn som används för att:

1. Inte uppdateras
2. Distribuera data jämnt, undvika data skeva
3. Minimera dataflyttning

### <a name="select-distribution-column-which-will-not-be-updated"></a>Välj distributionsplatser kolumn som inte kommer att uppdateras
Distributionskolumner kan inte uppdateras, därför, markera en kolumn med statiska värden.  Om en kolumn måste toobe uppdateras, är vanligtvis inte kandidat bra distribution.  Det finns ett fall där du måste uppdatera en kolumn för distribution, kan du göra det genom att först ta bort hello rad och sedan lägga till en ny rad.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Välj distributionsplatser kolumn som ska distribuera data jämnt
Eftersom ett distribuerat system utför bara så snabbt som motsvarande långsammaste distribution, är det viktigt toodivide hello arbete jämnt över hello distributioner ordning tooachieve belastningsutjämnade körning över hello system.  hello är hello arbete är uppdelad i ett distribuerat system utifrån där hello data för varje distribution finns.  Detta gör det mycket viktigt tooselect hello rätt distribution kolumnen för att distribuera hello data så att varje distribution har samma arbete och ska vidta hello samma tid toocomplete sin del av hello arbete.  När arbetet fördelas väl på hello system, fördelas hello data över hello-distributioner.  När data inte är balanserade jämnt, vi kallar detta **data förskjutning**.  

toodivide data jämnt och undvika skeva data bör du hello följande när du väljer kolumnen distribution:

1. Markera en kolumn som innehåller ett stort antal distinkta värden.
2. Undvik att dela ut data på kolumner med några få distinkta värden.
3. Undvik att dela ut data på kolumner med en hög frekvens av null-värden.
4. Undvik att dela ut data på datumkolumnerna.

Eftersom varje värde är hashformaterats too1 av 60 distributioner, tooachieve jämn fördelning vill tooselect en kolumn som är mycket unik och innehåller mer än 60 unika värden.  tooillustrate, Överväg att fall där en kolumn enbart har 40 unika värden.  Om den här kolumnen har valts som hello distribution tangent skulle hello data för tabellen hamna på 40 distributioner mest lämnar 20 distributioner med inga data och ingen bearbetning toodo.  Däremot skulle hello andra 40 distributioner få mer arbete toodo att om hello data var jämnt fördelade över 60 distributioner.  Det här scenariot är ett exempel på data skeva.

I MPP-system väntar varje frågesteg på att alla distributioner toocomplete sin andel av hello arbete.  Om en distribution är att mer arbete än hello andra: hello resurs av hello andra distributioner är i stort sett gått förlorat bara väntar på hello upptagen distribution.  När arbetet inte är jämnt fördelade över alla distributioner, vi kallar detta **bearbetning skeva**.  Förskjutning av bearbetning kommer frågor toorun långsammare än om hello arbetsbelastningen kan vara jämnt fördelade över hello-distributioner.  Förskjutning av data kommer att leda tooprocessing skeva.

Undvik att dela ut på hög nullbar kolumn som hello nullvärden alla hamnar på hello samma distribution. Distribuera på ett datum kan också orsakas av skeva bearbetning eftersom alla data för ett visst datum hamnar på hello samma distribution. Om flera användare kör frågor alla filtrering på hello samma datum, och sedan endast 1 av hello 60 distributioner kommer att göra alla hello arbete eftersom ett speciellt datum ska på en distributionsplats. I det här scenariot körs sannolikt hello frågor 60 gånger långsammare än om hello data har sprida jämnt över alla hello-distributioner.

När det finns inga bra kandidat kolumner, Överväg att använda resursallokering som hello distributionsmetod.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Välj distributionsplatser kolumn som minimerar dataflyttning
Minimera dataflyttning genom att markera hello rätt distribution kolumn är en hello viktigaste strategier för att optimera prestanda för ditt SQL Data Warehouse.  Dataflyttning uppstår vanligen när tabellerna eller aggregeringar utförs.  Kolumner som används i `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` och `HAVING` satser alla gör för **bra** hash-kandidater för distribution.

Hello å andra sidan kolumner i hello `WHERE` satsen gör **inte** gör för bra hash-kolumnen kandidater eftersom de begränsa vilka distributioner delta i hello-frågan som orsakar bearbetning skeva.  Ett bra exempel på en kolumn som kan vara nära till hands toodistribute på, men ofta kan orsaka denna skeva bearbetning är en datumkolumn.

Generellt sett om du har två stora-faktatabeller är ofta ingår i en koppling du kommer att få hello de flesta prestanda genom att distribuera båda tabellerna på en av hello kopplingskolumner.  Om du har en tabell som inte är domänanslutna tooanother stora faktatabell leta toocolumns som är vanliga i hello `GROUP BY` satsen.

Det finns några viktiga villkor som måste vara uppfyllda tooavoid dataflyttning under en koppling:

1. hello tabeller som ingår i hello koppling måste vara hash distribueras vid **en** av hello kolumner som ingår i hello koppling.
2. hello datatyperna hello kopplingskolumner måste matcha mellan båda tabellerna.
3. hello-kolumner måste vara ansluten med en equals-operatorn.
4. hello kopplingstyp får inte vara en `CROSS JOIN`.

## <a name="troubleshooting-data-skew"></a>Felsökningsdata skeva
När tabelldata distribueras med hjälp av hello hash distributionssätt finns förvrängd en risk att vissa distributioner kommer att toohave oproportionerligt mer data än andra. Onödigt stora datamängder skeva kan påverka prestanda för frågor eftersom hello slutresultatet av distribuerade frågor måste vänta på hello längsta körs distribution toofinish. Beroende på hello graden av hello data skeva måste du kanske tooaddress den.

### <a name="identifying-skew"></a>Identifiera förskjutning
Ett enkelt sätt tooidentify en tabell som förvrängd är toouse `DBCC PDW_SHOWSPACEUSED`.  Det här är en mycket snabbt och enkelt sätt toosee hello antalet rader som lagras i varje hello 60 distributioner av databasen.  Kom ihåg att hello mest belastningsutjämnade prestanda hello rader i tabellen distribuerade ska spridas jämnt över alla hello-distributioner.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Om du frågar hello Azure SQL Data Warehouse dynamiska hanteringsvyer (DMV) kan du utföra en mer detaljerad analys.  toostart, skapa hello vy [dbo.vTableSizes] [ dbo.vTableSizes] visas med hjälp av hello SQL från [tabell översikt] [ Overview] artikel.  Kör den här frågan tooidentify vilka tabeller har mer än 10% data förskjutning när hello vyn har skapats.

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Matcha data skeva
Inte alla skeva är tillräckligt med toowarrant en korrigering.  I vissa fall uppväger hello prestanda för en tabell i några frågor hello skada data skeva.  toodecide om du ska lösa data skeva i en tabell, bör du känna till så mycket som möjligt om hello datavolymer och frågor i din arbetsbelastning.   Enkelriktade toolook på hello effekten av förskjutning är toouse hello stegen i hello [frågan övervakning] [ Query Monitoring] artikel toomonitor hello effekten av skeva på frågeprestanda och specifikt hello påverkan toohow långa frågor ta toocomplete på enskilda hello-distributioner.

Distribuera data är en fråga om att hitta hello rätt balans mellan minimera förskjutning av data och minimera dataflyttning. Dessa kan motverkar mål och ibland vill du tookeep data skeva i ordning tooreduce dataflyttning. Till exempel när hello distribution kolumnen är ofta hello delade kolumn i kopplingar och aggregeringar, kommer du att minimera dataflyttning. hello kan fördelen med att ha hello minimal dataflyttning uppväger hello effekten av att data skeva.

hello vanligt tooresolve data förskjutning är toore-skapa hello tabell med en annan distributionsplats-kolumn. Eftersom det inte finns något sätt toochange hello distribution kolumn i en befintlig tabell, hello sätt toochange hello distribution av en tabell den toorecreate den med en [CTAS] [].  Här är två exempel på hur lösa skeva data:

### <a name="example-1-re-create-hello-table-with-a-new-distribution-column"></a>Exempel 1: Skapa nytt hello tabell med en ny kolumn för distribution
Det här exemplet används [CTAS] [] toore-skapa en tabell med en annan hash-distribution kolumn.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] too[FactInternetSales];
```

### <a name="example-2-re-create-hello-table-using-round-robin-distribution"></a>Exempel 2: Skapa nytt hello tabell med resursallokering (round robin)
Det här exemplet används [CTAS] [] toore-skapa en tabell med resursallokering i stället för en hash-distribution. Den här ändringen ger även Datadistribution hello kostnad för ökad dataflyttning.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] too[FactInternetSales];
```

## <a name="next-steps"></a>Nästa steg
toolearn mer om tabelldesign, se hello [fördela][Distribute], [Index][Index], [Partition] [ Partition], [Datatyper][Data Types], [statistik] [ Statistics] och [temporära tabeller] [ Temporary] artiklar.

En översikt över bästa praxis, se [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
