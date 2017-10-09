---
title: "aaaOptimizing transaktioner för SQL Data Warehouse | Microsoft Docs"
description: "Bästa praxis riktlinjer för att skriva effektiva transaktionsuppdateringar i Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 6f326f26-8a54-49df-a482-9c96a58db371
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 1a821161711db9460b7e10d3cf7ba498d711448b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimera transaktioner för SQL Data Warehouse
Den här artikeln förklarar hur toooptimize hello prestanda för transaktionell koden och minimerar risken för lång återställningar.

## <a name="transactions-and-logging"></a>Transaktioner och loggning
Transaktioner är en viktig komponent i en relationsdatabas-motor. SQL Data Warehouse använder transaktioner under dataändringar. Dessa transaktioner kan vara uttryckliga eller underförstådda. Enskild `INSERT`, `UPDATE` och `DELETE` -satser är alla exempel implicit transaktioner. Skrivs explicita transaktioner explicit av en utvecklare som använder `BEGIN TRAN`, `COMMIT TRAN` eller `ROLLBACK TRAN` och används vanligtvis när flera ändringar instruktioner måste toobe knutna tillsammans i en atomisk enhet. 

Azure SQL Data Warehouse genomför ändringar toohello databasen med hjälp av transaktionsloggar. Varje distribution har sin egen transaktionsloggen. Transaktionen sparas loggen automatiskt. Det finns ingen konfiguration krävs. Dock samtidigt som den här processen garanterar hello skrivåtgärder införs en kostnader i hello system. Du kan minimera den här effekten genom att skriva ett effektivt kod. Transaktionellt effektiv kod hamnar brett i två kategorier.

* Använd minimal loggning konstruktioner där det är möjligt
* Bearbetning av data med hjälp av omfång batchar tooavoid enda tidskrävande transaktioner
* Anta en partitionsväxling mönster för stora ändringar tooa angivna partition

## <a name="minimal-vs-full-logging"></a>Minimal kontra fullständig loggning
Till skillnad från fullständigt loggade åtgärder som använder hello transaction log tookeep reda på varje rad ändring, hålla minimalt loggade åtgärder reda på utsträckning allokeringar och endast metadata ändringar. Därför minimal loggning innebär att loggning endast hello-information som är nödvändiga toorollback hello transaktion i hello händelse av ett fel eller en uttrycklig begäran (`ROLLBACK TRAN`). Som mycket mindre information spåras i hello transaktionsloggen, presterar minimalt loggade åtgärden bättre än på samma sätt storlek fullständigt loggade åtgärden. Dessutom eftersom färre skrivningar går hello transaktionsloggen, en mycket mindre mängd loggdata genereras och det är flera i/o effektivt.

hello transaktion säkerhetsgränsen gäller endast toofully loggade åtgärder.

> [!NOTE]
> Minimalt loggade åtgärder kan delta i explicita transaktioner. Alla ändringar i allokering strukturer spåras, är det möjligt tooroll tillbaka minimalt loggade åtgärder. Det är viktigt toounderstand hello ändringen har ”minimalt” loggade den inte är icke loggas.
> 
> 

## <a name="minimally-logged-operations"></a>Minimalt loggade åtgärder
hello kan följande åtgärder loggas minimalt:

* SKAPA TABLE AS SELECT ([CTAS][CTAS])
* INSERT... VÄLJ
* SKAPA INDEX
* ALTER INDEX REBUILD
* DROP INDEX
* TRUNKERA TABELLEN
* SLÄPPA TABELLEN
* ALTER TABLE SWITCH PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> Internt dataflyttsåtgärderna (exempelvis `BROADCAST` och `SHUFFLE`) påverkas inte av hello transaktion säkerhet gränsen.
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>Minimal loggning med massinläsning
`CTAS`och `INSERT...SELECT` är både Massredigera belastningen åtgärder. Dock både påverkas av hello mål tabelldefinitionen och beror på hello belastningen scenario. Nedan finns en tabell med information om din massåtgärd helt eller minimalt loggas:  

| Primärt Index | Läs in Scenario | Loggningsläge |
| --- | --- | --- |
| Heap |Alla |**Minimal** |
| Grupperat Index |Tom måltabellen |**Minimal** |
| Grupperat Index |Inlästa rader inte överlappa befintliga sidor i mål |**Minimal** |
| Grupperat Index |Inlästa rader överlappar befintliga sidor i mål |Fullständig |
| Grupperat Columnstore-Index |Batchstorlek > = 102,400 per partition justerad distribution |**Minimal** |
| Grupperat Columnstore-Index |Batch-storlek < 102,400 per partition justerad distribution |Fullständig |

Det är värt att nämna att alla skrivningar tooupdate sekundär eller icke-grupperade index kommer alltid att vara helt loggade åtgärder.

> [!IMPORTANT]
> SQL Data Warehouse har 60-distributioner. Därför, förutsatt att alla rader fördelas jämnt och hamnar i en enda partition din batch måste toocontain 6,144,000 rader eller större toobe minimalt loggas när du skriver tooa klustrade Kolumnlagringsindexet. Om hello tabellen är partitionerad och hello rader infogas span partitionsgränser, behöver du 6,144,000 rader per partition gräns förutsatt att data fördelas jämnt. Varje partition i varje distribution måste oberoende överskrids hello 102 400 raden för hello insert toobe minimalt loggat in hello-distribution.
> 
> 

Läsa in data i en icke-tom tabell med ett grupperat index innehåller ofta en blandning av fullständigt loggade och minimalt loggade rader. Ett grupperat index är ett belastningsutjämnade träd (b-trädet) sidor. Om hello sida skrivs tooalready innehåller rader från en annan transaktion, sedan loggas dessa skrivningar fullständigt. Men om hello sida är tom loggas sedan hello skrivåtgärder toothat sidan minimalt.

## <a name="optimizing-deletes"></a>Optimera borttagningar
`DELETE`är fullständigt loggade åtgärden.  Om du behöver toodelete en stor mängd data i en tabell eller en partition kan det ofta passar bättre för`SELECT` hello data du vill tookeep som kan köras som ett minimalt loggade åtgärden.  tooaccomplish, skapa en ny tabell med [CTAS][CTAS].  När du skapat använda [Byt namn på] [ RENAME] tooswap ut din gamla tabell med hello nyskapad tabell.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only hello records we want tookep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename hello Tables tooreplace hello 
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] too[FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimera uppdateringar
`UPDATE`är fullständigt loggade åtgärden.  Om du behöver tooupdate ett stort antal rader i en tabell eller en partition det ofta vara mycket mer effektivt toouse minimalt loggade åtgärden som [CTAS] [ CTAS] toodo så.

I hello exempel under en fullständig uppdatering har konverterade tooa `CTAS` så att minimal loggning är möjligt.

I det här fallet vi efterhand lägger till en rabatt toohello försäljning i hello tabell:

```sql
--Step 01. Create a new table containing hello "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] too[FactInternetSales];

--Step 03. Drop hello old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> Skapa nytt stora tabeller kan dra nytta av funktioner för SQL Data Warehouse arbetsbelastning. För mer information finns toohello arbetsbelastning management-avsnittet i hello [samtidighet] [ concurrency] artikel.
> 
> 

## <a name="optimizing-with-partition-switching"></a>Optimera med Växla partition
När inför storskaliga ändringar i en [tabell partition][table partition], och sedan en partitionsväxling mönstret gör mycket bra. Om hello dataändring är viktig och spänner över flera partitioner, bara iterera över hello partitioner uppnår hello samma resultat.

hello steg tooperform en partition växel är följande:

1. Skapa en tom av partition
2. Utför hello uppdatering som en CTAS
3. Byta ut hello befintliga data toohello ut tabell
4. Växla i hello nya data
5. Rensa hello data

Dock identifiera toohelp hello partitioner tooswitch vi måste först toobuild en helper-procedur som hello en nedan. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id] 
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Den här proceduren maximerar återanvändning av kod och behåller hello partitionsväxling exempel mer kompakt.

hello koden nedan visar hello fem steg som nämns ovan tooachieve en fullständig partitionsväxling rutinen.

```sql
--Create a partitioned aligned empty table tooswitch out hello data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update hello data in hello select portion of hello CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use hello helper procedure tooidentify hello partitions
--hello source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--hello "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--hello "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch hello partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' too[dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' too[dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform hello clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Minimera loggning med små batchar
För stora mängder data ändras, kan det vara klokt toodivide hello åtgärden i segment eller batchar tooscope hello arbete.

Ett fungerande exempel finns nedan. hello batchstorlek har ställts in tooa trivial nummer toohighlight hello tekniken. I verkligheten är hello batchstorlek betydligt större. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Pausa och skalning vägledning
Azure SQL Data Warehouse kan du pausa, fortsätta och skala ditt informationslager på begäran. När du pausar du eller skala ditt SQL Data Warehouse är det viktigt toounderstand att alla pågående transaktioner avslutas omedelbart. orsakar toobe alla öppna transaktioner återställs. Om din arbetsbelastning har utfärdats tidskrävande och ofullständiga data ändras tidigare måste toohello paus eller skalningsåtgärden sedan detta verk toobe ångra. Detta kan påverka hello tid det tar toopause eller skala din Azure SQL Data Warehouse-databas. 

> [!IMPORTANT]
> Båda `UPDATE` och `DELETE` är fullständigt loggade åtgärder och så att dessa Ångra/Gör om kan ta betydligt längre tid än motsvarande minimalt loggade åtgärder. 
> 
> 

hello bästa scenario är toolet i svarta data ändras transaktioner fullständig tidigare toopausing eller skalning SQL Data Warehouse. Men kan det inte alltid praktiskt. toomitigate hello risken för en lång återställning betrakta ett av hello följande alternativ:

* Skriv långvariga åtgärder med hjälp av [CTAS][CTAS]
* Nedbrytning hello åtgärden i segment; operativsystem för en delmängd av hello rader

## <a name="next-steps"></a>Nästa steg
Se [transaktioner i SQL Data Warehouse] [ Transactions in SQL Data Warehouse] toolearn mer om isoleringsnivåer och transaktionell gränser.  En översikt över andra metodtips finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Transactions in SQL Data Warehouse]: ./sql-data-warehouse-develop-transactions.md
[table partition]: ./sql-data-warehouse-tables-partition.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

