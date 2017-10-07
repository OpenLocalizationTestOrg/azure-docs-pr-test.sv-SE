---
title: aaaCreate surrogat nycklar identiteten | Microsoft Docs
description: "Lär dig hur toouse identitet toocreate surrogat nycklar på tabeller."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/13/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 502cdd2b510b229b2a19c1f78b11862a7386ae3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a>Skapa surrogat nycklar identiteten
> [!div class="op_single_selector"]
> * [Översikt över][Overview]
> * [Datatyper][Data Types]
> * [Distribuera][Distribute]
> * [Index][Index]
> * [Partition][Partition]
> * [Statistik][Statistics]
> * [Tillfällig][Temporary]
> * [Identitet][Identity]
> 
> 

Många data Modellerare som toocreate surrogat nycklar på deras tabeller när de skapar data warehouse-modeller. Du kan använda hello identitet egenskapen tooachieve det här målet enkelt och effektivt sätt utan att läsa in prestanda påverkas. 

## <a name="get-started-with-identity"></a>Kom igång med identitet
Du kan definiera en tabell med hello IDENTITY-egenskapen när du skapar hello tabellen med hjälp av syntax liknande toohello följande instruktion:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Du kan sedan använda `INSERT..SELECT` toopopulate hello tabell.

## <a name="behavior"></a>Beteende
hello IDENTITY-egenskapen är utformad tooscale ut över alla hello distributioner i hello data warehouse utan att läsa in prestanda påverkas. Därför är hello implementering av IDENTITETEN riktade mot att uppnå dessa mål. Det här avsnittet visar hello olika delarna av hello implementering toohelp du förstår dem. mer i detalj.  

### <a name="allocation-of-values"></a>Allokering av värden
Hej identitetsegenskap garanterar inte hello-ordning i vilken hello allokeras surrogat värden, som visar hello beteendet för SQL Server och Azure SQL Database. Men i Azure SQL Data Warehouse är hello frånvaron av en garanti tydligare. 

hello följande exempel är en bild:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

I föregående exempel hello, landat två rader i distribution 1. hello första raden har hello surrogat värdet 1 i kolumnen `C1`, och hello andra raden har hello surrogat värde 61. Båda dessa värden har genererats av hello IDENTITY-egenskapen. Hello allokering av hello värden är inte sammanhängande. Det här beteendet är avsiktligt.

### <a name="skewed-data"></a>Skeva data 
hello värdeintervallet för hello datatyp jämnt fördelade över hello-distributioner. Om en distribuerad tabell drabbas från skeva data, hello du värdeintervallet tillgängliga toohello datatype slut för tidigt. Till exempel om alla hello data avslutas med en enda distributionsplats, sedan effektivt hello tabellen har åtkomst tooonly 1-/ 60 hello värden hello datatyp. Därför hello IDENTITY-egenskapen är begränsad för`INT` och `BIGINT` datatyperna endast.

### <a name="selectinto"></a>VÄLJ... I
När en befintlig identitetskolumn väljs i en ny tabell, ärver hello ny kolumn hello IDENTITY-egenskapen, såvida inte någon av hello följande villkor är uppfyllda:
- hello SELECT-satsen innehåller en koppling.
- Flera SELECT-satser kopplas med hjälp av UNION.
- hello identitetskolumnen visas mer än en gång i hello SELECT-listan.
- hello IDENTITY-kolumn är en del av ett uttryck.
    
Om någon av dessa villkor är SANT skapas hello kolumn inte är NULL i stället för att ärva hello IDENTITY-egenskapen.

### <a name="create-table-as-select"></a>SKAPA TABLE AS SELECT
Skapa tabell AS Välj (CTAS) följer hello samma SQLServer-beteende som beskrivs i SELECT... I. Men du kan inte ange en IDENTITY-egenskapen i hello kolumndefinition i hello `CREATE TABLE` tillhör hello-instruktion. Du kan också använda hello IDENTITY-funktionen i hello `SELECT` tillhör hello CTAS. toopopulate en tabell, behöver du toouse `CREATE TABLE` toodefine hello tabell följt av `INSERT..SELECT` toopopulate den.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Uttryckligen lägga till värden i en identitetskolumn 
SQL Data Warehouse stöder `SET IDENTITY_INSERT <your table> ON|OFF` syntax. Du kan använda den här syntaxen tooexplicitly infoga värden i hello identitetskolumn.

Många data Modellerare som toouse fördefinierade negativa värden för vissa rader i sina dimensioner. Ett exempel är hello -1 eller ”okänd medlem” rad. 

hello nästa skript visar hur tooexplicitly lägger till den här raden med hjälp av ange IDENTITY_INSERT:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a>Läs in data i en tabell med identitet

hello förekomst av hello IDENTITY-egenskapen har effekter tooyour datainläsning kod. Det här avsnittet beskrivs vissa grundläggande mönster för att läsa in data i tabeller identiteten. 

### <a name="load-data-with-polybase"></a>Läs in data med PolyBase
tooload data i en tabell och skapa surrogat nyckel identiteten, skapa hello tabell och sedan använda INSERT... Välj eller infoga... VÄRDEN tooperform hello belastning.

hello visar följande exempel hello grundläggande mönster:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT toopopulate hello table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Det är inte möjligt toouse `CREATE TABLE AS SELECT` för närvarande vid inläsning av data i en tabell med en identitetskolumn.
> 

Mer information om att läsa in data med verktyget hello bulk copy program (BCP) finns i hello följande artiklar:

- [Läsa in med PolyBase][]
- [PolyBase bästa praxis][]

### <a name="load-data-with-bcp"></a>Läs in data med BCP
BCP är ett kommandoradsverktyg som du kan använda tooload data till SQL Data Warehouse. En av dess parametrar (-E) kontroller hello beteendet för BCP vid inläsning av data i en tabell med en identitetskolumn. 

Om du har angett -E värde hello lagras i hello indatafilen för hello kolumn med IDENTITETEN. Om E - *inte* anges ignoreras hello värden i den här kolumnen. Om hello identity-kolumn inte är inkluderad sedan hello data har lästs in som vanligt. hello värden genereras enligt toohello ökning och seed princip för hello-egenskapen.

Mer information om att läsa in data med hjälp av BCP finns hello följande artiklar:

- [Läsa in med BCP][]
- [BCP i MSDN][]

## <a name="catalog-views"></a>Katalogvyer
SQL Data Warehouse stöder hello `sys.identity_columns` vyn katalog. Den här vyn kan vara används tooidentify en kolumn som innehåller hello IDENTITY-egenskapen.

toohelp bättre förstå hello databasschemat, det här exemplet illustrerar hur toointegrate `sys.identity_columns` med andra system katalogvyer:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Begränsningar
hello IDENTITY-egenskapen kan inte användas i hello följande scenarier:
- Om hello Kolumndatatypen inte är INT eller BIGINT
- Där hello kolumnen är också hello distribution
- Där hello tabell är en extern tabell 

hello följande relaterade funktioner stöds inte i SQL Data Warehouse:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Uppgifter

Det här avsnittet innehåller även kodexempel du kan använda tooperform vanliga uppgifter när du arbetar med identitetskolumner.

> [!NOTE] 
> Kolumnen C1 är hello identitet i alla hello följande uppgifter.
> 
 
### <a name="find-hello-highest-allocated-value-for-a-table"></a>Hitta hello högsta allokerade värde för en tabell
Använd hello `MAX()` fungerar toodetermine hello högsta värde som tilldelas för en distribuerad tabell:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-hello-seed-and-increment-for-hello-identity-property"></a>Hitta hello start- och ökningsvärden för hello IDENTITY-egenskapen
Du kan använda hello katalog vyer toodiscover hello identitet ökning och seed configuration värden för en tabell med hjälp av hello följande fråga: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Nästa steg

* toolearn mer information om hur du utvecklar tabeller, se [tabell översikt][Overview], [tabell datatyper][Data Types], [distribuera en tabell] [ Distribute], [Index i en tabell][Index], [partitionera en tabell][Partition], och [ Temporära tabeller][Temporary]. 
* Mer information om metodtips finns [SQL Data Warehouse metodtips][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[Läsa in med bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[Läsa in med PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[PolyBase bästa praxis]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[BCP i MSDN]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  
