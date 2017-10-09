---
title: "aaaGuide för att använda PolyBase i SQL Data Warehouse | Microsoft Docs"
description: "Riktlinjer och rekommendationer för att använda PolyBase i SQL Data Warehouse-scenarier."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 6/5/2016
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: b05e4c5d528f2fe1c60d6855b5333065f0c908ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Guide för att använda PolyBase i SQL Data Warehouse
Den här handboken innehåller praktisk information för att använda PolyBase i SQL Data Warehouse.

tooget igång, se hello [Läs in data med PolyBase] [ Load data with PolyBase] kursen.

## <a name="rotating-storage-keys"></a>Rotera lagringsnycklar
Från tid tootime ska toochange hello access key tooyour blob-lagring av säkerhetsskäl.

Hej mest elegant sätt tooperform uppgiften är toofollow en process som kallas ”rotera nycklar hello”. Kanske såg du att du har två lagringsnycklar för blob storage-konto. Detta är så att du kan överföra

Rotera dina nycklar för Azure storage-konto är tre enkla steg

1. Skapa andra omfång databasautentiseringsuppgiften baserat på hello sekundära lagringsåtkomstnyckel
2. Skapa extern datakälla baserat på den här nya autentiseringsuppgifter
3. Ta bort och skapa hello externa tabeller pekar toohello nya extern datakälla

När du har migrerat alla externa tabeller toohello nya externa datakällan och sedan kan du utföra hello Rensa uppgifter:

1. Släpp första extern datakälla
2. Släpp första databas-omfattande autentisering baserat på hello primära lagringsåtkomstnyckel
3. Logga in på Azure och återskapa hello primärnyckeln redo för hello nästa gång

## <a name="query-azure-blob-storage-data"></a>Frågan Azure blob storage-data
Frågor mot externa tabeller Använd bara hello tabellnamn som om det var en relationella tabell.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> En fråga på en extern tabell kan misslyckas med hello fel *”frågan avbröts--hello avvisa den maximala tröskelvärdet nåddes vid läsning från en extern källa”*. Detta anger att externa data innehåller *felaktig* poster. ”Felaktig' anses vara en post om hello faktiska data typer/antal kolumner inte matchar hello kolumndefinitioner för hello extern tabell eller om hello data stämmer inte överens toohello angivna externa filformatet. toofix, se till att en extern tabell och definitioner för externa filformat är korrekta och externa data överensstämmer toothese definitioner. Om en delmängd av externa dataposter är inaktuella kan du välja tooreject posterna för dina frågor med hello avvisa Skapa extern tabell DDL.
> 
> 

## <a name="load-data-from-azure-blob-storage"></a>Läsa in data från Azure Blob Storage
Det här exemplet läser in data från Azure blob storage tooSQL Data Warehouse-databas.

Lagra data direkt tar bort hello dataöverföringstid för frågor. Lagra data med ett columnstore-index förbättrar prestanda för analys av in too10x.

Det här exemplet använder hello CREATE TABLE AS SELECT-instruktionen tooload data. hello ny tabell ärver hello kolumnerna som namnges i frågan hello. Hello datatyperna för kolumnerna ärver från hello externa tabelldefinitionen.

CREATE TABLE AS SELECT är en hög performant Transact-SQL-uttryck som läser in hello data i parallella tooall hello compute-noder i SQL Data Warehouse.  Den ursprungligen utvecklades för hello massivt parallell bearbetning (MPP) motorn i Analytics Platform System och är nu i SQL Data Warehouse.

```sql
-- Load data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Se [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].

## <a name="create-statistics-on-newly-loaded-data"></a>Skapa statistik på nyinlästa data
SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.  I ordning tooget hello bästa prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data.  En detaljerad förklaring av statistik finns hello [statistik] [ Statistics] -avsnittet i hello ämnesgruppen.  Nedan visas ett enkelt exempel på hur toocreate statistik hello fram läses in i det här exemplet.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-tooazure-blob-storage"></a>Exportera data tooAzure blob-lagring
Det här avsnittet visar hur tooexport data från SQL Data Warehouse tooAzure blob storage. Det här exemplet använder skapa externa TABLE AS SELECT som är en hög performant Transact-SQL-instruktionen tooexport hello data parallellt från alla hello compute-noder.

hello skapas följande exempel en extern tabell Weblogs2014 med kolumndefinitionerna och data från dbo. Webbloggar tabell. hello externa tabelldefinitionen lagras i SQL Data Warehouse och hello hello SELECT-instruktionen är exporterade toohello ”/ Arkivera/log2014 /” katalog under hello blob-behållare som angavs av hello datakällan. hello data exporteras i hello angiven text-format.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a>Isolera läsa in användare
Det är ofta en toohave behovet av flera användare som kan läsa in data i en SQL DW. Eftersom hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] kräver behörighet av hello databasen du kommer att få flera användare med behörighet över alla scheman. toolimit, du kan använda hello NEKA kontroll.

Exempel: Överväg att databasen scheman schema_A för Avd A och schema_B för Avd B låt databasen användare user_A och user_B vara användare för PolyBase läser in i Avd A och B, respektive. De båda ha beviljats behörighet för databasen.
hello skapare av A och B nu låsa deras scheman med NEKA-schemat:

```sql
   DENY CONTROL ON SCHEMA :: schema_A toouser_B;
   DENY CONTROL ON SCHEMA :: schema_B toouser_A;
```   
 Med den här, user_A och user_B bör nu vara utelåst från hello andra Avd schemat.
 


## <a name="next-steps"></a>Nästa steg
toolearn mer om att flytta data tooSQL datalagret finns hello [översikt över datamigrering][data migration overview].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
