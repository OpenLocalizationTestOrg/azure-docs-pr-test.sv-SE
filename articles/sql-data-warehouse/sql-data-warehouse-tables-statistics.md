---
title: "aaaManaging statistik på tabellerna i SQL Data Warehouse | Microsoft Docs"
description: "Komma igång med statistik på tabellerna i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: c9521dc47891f68d124e77a53e2e15d03275caaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Hantera statistik på tabellerna i SQL Data Warehouse
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

hello mer SQL Data Warehouse medveten om dina data hello snabbare den kan köra frågor mot dina data.  hello sätt som du anger SQL Data Warehouse om dina data är genom att samla in statistik om dina data.  Med statistik om dina data är en av hello viktigaste saker du kan göra toooptimize dina frågor.  Statistik kan SQL Data Warehouse skapar hello mest optimala plan för dina frågor.  Det beror på att hello SQL Data Warehouse frågan optimering är en kostnad baserat optimering.  Det vill säga jämför hello kostnaden för olika frågeplaner och väljer hello plan med hello lägsta kostnad som bör också vara hello-plan som utför hello snabbaste.

Statistik kan skapas på en kolumn, flera kolumner eller i ett index i en tabell.  Statistik lagras i ett histogram som samlar in hello intervall och selektivitet värden.  Detta är särskilt intressanta när hello optimering behöver tooevaluate kopplingar, GROUP BY, HAVING och WHERE-satser i en fråga.  Till exempel hello optimering uppskattar att hello datum du filtrerar i frågan returnerar 1 rad, det kan välja väldigt annorlunda planera än om den beräknar de datum du har valt att returnera 1 miljon rader.  Även om det är mycket viktigt att statistik skapas, är det lika viktigt att statistik *korrekt* återge hello nuvarande hello tabell.  Uppdaterad statistik försäkrar du dig om att en bra plan är markerade som hello optimering.  hello planer som skapats av hello optimering är bättre hello statistik på dina data.

hello-processen för att skapa och uppdatera statistik för närvarande är en manuell process, men är väldigt enkelt toodo.  Detta skiljer sig från SQL Server som automatiskt skapar och uppdaterar statistik på enskild kolumner och index.  Du kan avsevärt automatisera hello hantering av hello statistik på dina data med hjälp av hello informationen nedan. 

## <a name="getting-started-with-statistics"></a>Komma igång med statistik
 Provade statistik skapas för varje kolumn är ett enkelt sätt tooget igång med statistik.  Eftersom det är lika viktigt tookeep statistik uppdaterade, en försiktig metod kan vara tooupdate statistiken varje dag eller efter varje inläsningen. Det finns alltid avvägningarna mellan prestanda och hello kostnaden toocreate och uppdatera statistik.  Om du hittar det tar för lång toomaintain alla din statistik kan kanske du vill tootry toobe mer noga med vilka kolumner har statistik eller vilka kolumner som behöver uppdateras regelbundet.  Du kan till exempel vill tooupdate datumkolumnerna dagliga som nya värden kan läggas till i stället för efter varje inläsningen. Igen och du kommer att få hello utnyttja genom att låta statistik för kolumner som ingår i kopplingar, GROUP BY, HAVING och WHERE-satser.  Om du har en tabell med många kolumner som används endast i hello Markera satsen statistik på dessa kolumner kan inte hjälpa och utgifter lite mer arbete tooidentify endast hello kolumner där statistik hjälper kan minska hello tid toomaintain din statistik .

## <a name="multi-column-statistics"></a>Flera kolumnstatistik
Dessutom toocreating statistik på enskild kolumner, kan vara att dina frågor att dra nytta av flera kolumnstatistik.  Flera kolumnstatistik är statistik skapas på en lista med kolumner.  De inkluderar kolumnstatistik hello första kolumnen i hello lista, plus vissa mellan kolumnen korrelation information kallas densitet.  Till exempel om du har en tabell som ansluter till tooanother på två kolumner kan vara att SQL Data Warehouse bättre kan optimera hello planen om den förstår hello relationen mellan två kolumner.   Flera kolumnstatistik kan förbättra frågeprestanda för vissa åtgärder, till exempel sammansatta kopplingar och gruppera efter.

## <a name="updating-statistics"></a>Uppdatera statistik
Uppdaterar statistiken är en viktig del av din databas management rutinen.  När hello fördelning av data i hello-databasen ändras, måste statistik toobe uppdateras.  Inaktuella statistik leder toosub optimala frågeprestanda.

Ett bra tips är tooupdate statistik över datumkolumnerna varje dag som nya datum har lagts till.  Varje gång nya rader läses in i datalagret hello, nya belastningen datum eller datum har lagts till. Dessa ändra hello datadistribution och se hello statistik inaktuella. Statistik i ett land-kolumn i en kundtabell behöva däremot aldrig toobe uppdateras, som vanligtvis inte ändras hello fördelningen av värden. Under förutsättning att hello distribution är konstant mellan kunder, enkelt lägga till nya rader toohello tabell variation toochange hello Datadistribution. Dock om ditt data warehouse endast innehåller ett land och du hämta data från ett nytt land, måste vilket resulterar i data från flera länder lagras, definitivt tooupdate statistik hello land kolumn.

En av hello första frågor tooask när felsökning av en fråga är ”är hello statistik uppdaterade”?

Den här frågan är inte en som kan betjänas av hello åldern på hello data. Ett dig toodate statistik objekt kan vara mycket gamla om det har förekommit några väsentlig förändring toohello underliggande data. När hello antalet rader som har ändrats betydligt eller så finns det en väsentlig förändring i hello fördelning av värden för en viss kolumn *sedan* är det tooupdate statistik.  

Referens **SQL Server** (inte SQL Data Warehouse) uppdateras automatiskt statistik för dessa situationer:

* Om du har noll rader i tabellen hello, när du lägger till rader, får du en automatisk uppdatering av statistik
* När du lägger till mer än 500 rader tooa tabell från och med färre än 500 rader (t.ex. vid start du har 499 och Lägg sedan till 500 rader tooa totalt 999 rader), får du en automatisk uppdatering 
* När du är än 500 rader har du tooadd 500 ytterligare rader + 20% av hello storlek hello tabell innan du ser en automatisk uppdatering på hello statistik

Eftersom det inte finns några DMV toodetermine om data i hello tabellen har ändrats sedan hello senaste statistik har uppdaterats, kan veta hello ålder på din statistik förse dig med en del av hello bild.  Du kan använda följande fråga toodetermine hello senast statistiken hello där uppdateras vid varje tabell.  

> [!NOTE]
> Kom ihåg att om det finns en väsentlig förändring i hello fördelning av värden för en kolumn, bör du uppdatera statistik oavsett hello senast som de har uppdaterats.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Datumkolumnerna i datalagret, till exempel måste vanligtvis frekventa uppdateringar av statistik. Varje gång nya rader läses in i datalagret hello, nya belastningen datum eller datum har lagts till. Dessa ändra hello datadistribution och se hello statistik inaktuella.  Däremot kanske aldrig statistik på kolumnen kön för en kundtabell som toobe uppdateras. Under förutsättning att hello distribution är konstant mellan kunder, enkelt lägga till nya rader toohello tabell variation toochange hello Datadistribution. Men om ditt data warehouse bara innehåller en kön och en ny krav resulterar i flera könen måste definitivt tooupdate statistik hello kolumnen för kön.

Ytterligare förklaring finns [statistik] [ Statistics] på MSDN.

## <a name="implementing-statistics-management"></a>Implementera hantering av statistik
Det är ofta en bra idé tooextend din datainläsning processen tooensure som uppdateras vid hello slutet av hello belastningen. hello datainläsning är när tabeller ändras oftast deras storlek och/eller distribution av värden. Därför är detta en logisk plats tooimplement vissa hanteringsprocesser.

Vissa riktlinjerna tillhandahålls nedan för att uppdatera statistiken under inläsningen hello:

* Kontrollera att varje inlästa tabell har minst ett statistik objekt uppdateras. Den här uppdateringar hello tabeller (radantal och antal sidor) Storleksinformation som en del av hello stats uppdateringen.
* Fokusera på kolumner som ingår i koppling, GROUP BY, ORDER BY och DISTINCT-satser
* Överväg att uppdatera ”stigande nyckeln” kolumner som transaktionen datum oftare, eftersom dessa värden inte inkluderas i hello statistik histogram.
* Överväg att uppdatera statiska distributionskolumner mindre ofta.
* Kom ihåg att varje statistik objekt uppdateras i serien. Bara implementera `UPDATE STATISTICS <TABLE_NAME>` kanske inte är optimal - särskilt för många tabeller med många statistik objekt.

> [!NOTE]
> Mer information om [stigande nyckeln] finns toohello SQL Server 2014 kardinalitet beräkning av modellen vitbok.
> 
> 

Ytterligare förklaring finns [kardinalitet uppskattning] [ Cardinality Estimation] på MSDN.

## <a name="examples-create-statistics"></a>Exempel: Skapa statistik
Följande exempel visar hur toouse olika alternativ för att skapa statistik. hello-alternativ som du använder för varje kolumn är beroende av hello egenskaper för dina data och hur hello kolumnen kommer att användas i frågor.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Skapa statistik för en kolumn med standardalternativ
toocreate statistik om en kolumn, ange bara ett namn för hello statistik objektet och hello hello kolumnens namn.

Den här syntaxen använder alla hello standardalternativ. Som standard prover SQL Data Warehouse 20 procent av hello tabellen när den skapar statistik.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Exempel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Skapa enkolumns-statistik genom att undersöka varje rad
hello standard samplingsfrekvensen 20 procent räcker för de flesta situationer. Du kan dock justera samplingsfrekvensen hello.

toosample hello fullständig tabell, använder du följande syntax:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Exempel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-hello-sample-size"></a>C. Skapa enkolumns-statistik genom att ange hello provtagning
Du kan också ange hello provtagning procent:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-hello-rows"></a>D. Skapa enkolumns-statistik på endast en del av hello rader
Ett annat alternativ du kan skapa statistik på en del av hello rader i tabellen. Detta kallas en filtrerad statistik.

Du kan till exempel använda filtrerad statistik när du planerar tooquery en specifik partition för en stor partitionerad tabell. Genom att skapa statistik på endast hello partition värden, kommer hello riktighet hello statistik förbättra och därför förbättra frågeprestanda.

Det här exemplet skapar statistik på ett intervall med värden. hello-värden kan enkelt definierats toomatch hello värdeintervallet i en partition.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> För hello frågan optimering tooconsider använder filtrerad statistik när de väljer hello distribuerade frågeplan, hello frågan måste få plats inuti hello definition av hello statistik objekt. Med hjälp av hello föregående exempel hello frågans där sats måste toospecify Kol1 värden mellan 2000101 och 20001231.
> 
> 

### <a name="e-create-single-column-statistics-with-all-hello-options"></a>E. Skapa enkolumns-statistik med alla hello-alternativ
Du kan självklart kombinera hello alternativ tillsammans. hello exemplet nedan skapar en filtrerad statistik-objekt med en anpassad provtagning:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Hello fullständiga referenser finns [CREATE STATISTICS] [ CREATE STATISTICS] på MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Skapa flera kolumner statistik
toocreate flera kolumnstatistik, kan du bara använda hello föregående exempel, men ange fler kolumner.

> [!NOTE]
> hello histogram som används för tooestimate antalet rader i hello frågeresultat är endast tillgängligt för hello första kolumnen i hello statistik objektdefinition.
> 
> 

I det här exemplet hello histogram finns på *produkten\_kategori*. Statistik för Cross-kolumnen beräknas på *produkten\_kategori* och *produkten\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Eftersom det inte finns en korrelation mellan *produkten\_kategori* och *produkten\_sub\_kategori*, en flera kolumner kan vara användbart om de här kolumnerna kan nås vid hello samtidigt.

### <a name="g-create-statistics-on-all-hello-columns-in-a-table"></a>G. Skapa statistik på alla hello kolumner i en tabell
Enkelriktade toocreate statistik är tooissues CREATE STATISTICS kommandon när du har skapat hello tabell.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-toocreate-statistics-on-all-columns-in-a-database"></a>H. Använda en lagrad procedur toocreate statistik på alla kolumner i en databas
SQL Data Warehouse inte har en motsvarighet på system lagrade proceduren för [sp_create_stats] [] i SQL Server. Den här lagrade proceduren skapar en kolumn statistik-objekt på varje kolumn i hello-databas som inte redan har statistik.

Detta hjälper dig att komma igång med din databasdesign. Känna sig fria tooadapt den tooyour måste.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

toocreate statistik på alla kolumner i tabellen hello med den här proceduren anropa bara hello-procedur.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Exempel: uppdatera statistik
tooupdate statistik, kan du:

1. Uppdatera ett objekt av statistik. Ange hello statistik objekt du vill tooupdate hello namn.
2. Uppdatera alla statistik objekt i en tabell. Ange hello namn hello tabellen i stället för en viss statistik-objektet.

### <a name="a-update-one-specific-statistics-object"></a>A. Uppdatera en specifik statistik objekt
Använd hello följande syntax tooupdate en viss statistik-objekt:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Exempel:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Du kan minimera hello tid och resurser krävs toomanage statistik genom att uppdatera statistik för specifika objekt. Detta kräver vissa tänkte, men toochoose hello bästa statistik objekt tooupdate.

### <a name="b-update-all-statistics-on-a-table"></a>B. Uppdatera all statistik för en tabell
Detta visar en enkel metod för att uppdatera alla hello statistik objekt i en tabell.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Exempel:

```sql
UPDATE STATISTICS dbo.table1;
```

Den här instruktionen är enkelt toouse. Kom ihåg detta uppdaterar all statistik för hello tabellen och därför kan utföra mer arbete än nödvändigt. Om hello prestanda inte är ett problem, men det är definitivt hello enklaste och sätt tooguarantee statistik är uppdaterade.

> [!NOTE]
> När du uppdaterar all statistik för en tabell, stöder SQL Data Warehouse en genomsökning toosample hello tabell för varje statistik. Om hello tabell är stort, har många kolumner och många statistik, kan det vara effektivare tooupdate-enskilda statistik baserat på behov.
> 
> 

För en implementering av en `UPDATE STATISTICS` proceduren finns hello [temporära tabeller] [ Temporary] artikel. hello implementering metoden är något annorlunda toohello `CREATE STATISTICS` proceduren ovan men hello slutresultatet är hello samma.

Hello fullständig syntax, se [Update Statistics] [ Update Statistics] på MSDN.

## <a name="statistics-metadata"></a>Statistik metadata
Det finns flera systemvy och funktioner som du kan använda toofind information om statistik. Du kan till exempel se om ett statistik-objekt kan vara inaktuell med hello stats datum funktionen toosee när statistiken skapades eller uppdaterades senast.

### <a name="catalog-views-for-statistics"></a>Katalogvyer för statistik
Dessa systemvyer innehåller information om statistik:

| Vyn katalog | Beskrivning |
|:--- |:--- |
| [sys.Columns][sys.columns] |En rad för varje kolumn. |
| [sys.Objects][sys.objects] |En rad för varje objekt i hello-databasen. |
| [sys.schemas][sys.schemas] |En rad för varje schema i hello-databasen. |
| [sys.stats][sys.stats] |En rad för varje objekt i statistik. |
| [sys.stats_columns][sys.stats_columns] |En rad för varje kolumn i hello statistik för objektet. Länkar tillbaka toosys.columns. |
| [sys.Tables][sys.tables] |En rad för varje tabell (inklusive externa tabeller). |
| [sys.table_types][sys.table_types] |En rad för varje datatyp. |

### <a name="system-functions-for-statistics"></a>Systemfunktioner för statistik
Dessa systemfunktioner är användbara för att arbeta med statistik:

| Systemfunktionen | Beskrivning |
|:--- |:--- |
| [STATS_DATE][STATS_DATE] |Datum hello statistik objekt uppdaterades senast. |
| [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] |Innehåller översiktlig nivå och detaljerad information om hello fördelningen av värden som tolkas av hello statistik-objektet. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Kombinera statistik kolumner och funktioner i en vy
Den här vyn visar kolumner som rör toostatistics och resultat från hello [STATS_DATE()] [] funktionen tillsammans.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS() exempel
DBCC SHOW_STATISTICS() visar hello data som lagras i ett statistik-objekt. Dessa data finns i tre delar.

1. Huvudet
2. Densitet Vector
3. Histogram

hello sidhuvud metadata om hello statistik. hello histogram visar hello fördelningen av värden i hello första nyckelkolumn hello statistik för objektet. Hej densitet vector åtgärder mellan kolumnen korrelation. SQLDW beräknar kardinalitet uppskattningar med någon av hello data i hello statistik för objektet.

### <a name="show-header-density-and-histogram"></a>Visa sidhuvud, densitet och histogram
Det här enkla exemplet visar alla tre delar av ett objekt av statistik.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Exempel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Visa en eller flera delar av DBCC SHOW_STATISTICS();
Om du bara vill visa specifika delar använder hello `WITH` satsen och ange vilka delar du vill toosee:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Exempel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() skillnader
DBCC SHOW_STATISTICS() implementeras striktare i SQL Data Warehouse jämfört med tooSQL Server.

1. Odokumenterade funktioner stöds inte
2. Det går inte att använda Stats_stream
3. Kan inte ansluta resultat för specifika delmängder av statistikdata, t.ex. (STAT_HEADER koppling DENSITY_VECTOR)
4. NO_INFOMSGS kan inte anges för Undertryckning meddelande
5. Hakparenteser runt statistik namn kan inte användas
6. Det går inte att använda kolumnen namn tooidentify statistik objekt
7. Anpassade fel 2767 stöds inte

## <a name="next-steps"></a>Nästa steg
Mer information finns i [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] på MSDN.  toolearn finns fler hello artiklar på [tabell översikt][Overview], [Data tabelltyper][Data Types], [distribuerar en tabell] [ Distribute], [Indexering av en tabell][Index], [partitionering en tabell] [ Partition] och [ Temporära tabeller][Temporary].  Mer information om metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].  

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

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
