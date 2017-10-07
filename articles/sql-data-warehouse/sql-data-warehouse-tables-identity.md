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
# <a name="create-surrogate-keys-by-using-identity"></a><span data-ttu-id="6c566-103">Skapa surrogat nycklar identiteten</span><span class="sxs-lookup"><span data-stu-id="6c566-103">Create surrogate keys by using IDENTITY</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="6c566-104">[Översikt över][Overview]</span><span class="sxs-lookup"><span data-stu-id="6c566-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="6c566-105">[Datatyper][Data Types]</span><span class="sxs-lookup"><span data-stu-id="6c566-105">[Data types][Data Types]</span></span>
> * <span data-ttu-id="6c566-106">[Distribuera][Distribute]</span><span class="sxs-lookup"><span data-stu-id="6c566-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="6c566-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="6c566-107">[Index][Index]</span></span>
> * <span data-ttu-id="6c566-108">[Partition][Partition]</span><span class="sxs-lookup"><span data-stu-id="6c566-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="6c566-109">[Statistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="6c566-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="6c566-110">[Tillfällig][Temporary]</span><span class="sxs-lookup"><span data-stu-id="6c566-110">[Temporary][Temporary]</span></span>
> * <span data-ttu-id="6c566-111">[Identitet][Identity]</span><span class="sxs-lookup"><span data-stu-id="6c566-111">[Identity][Identity]</span></span>
> 
> 

<span data-ttu-id="6c566-112">Många data Modellerare som toocreate surrogat nycklar på deras tabeller när de skapar data warehouse-modeller.</span><span class="sxs-lookup"><span data-stu-id="6c566-112">Many data modelers like toocreate surrogate keys on their tables when they design data warehouse models.</span></span> <span data-ttu-id="6c566-113">Du kan använda hello identitet egenskapen tooachieve det här målet enkelt och effektivt sätt utan att läsa in prestanda påverkas.</span><span class="sxs-lookup"><span data-stu-id="6c566-113">You can use hello IDENTITY property tooachieve this goal simply and effectively without affecting load performance.</span></span> 

## <a name="get-started-with-identity"></a><span data-ttu-id="6c566-114">Kom igång med identitet</span><span class="sxs-lookup"><span data-stu-id="6c566-114">Get started with IDENTITY</span></span>
<span data-ttu-id="6c566-115">Du kan definiera en tabell med hello IDENTITY-egenskapen när du skapar hello tabellen med hjälp av syntax liknande toohello följande instruktion:</span><span class="sxs-lookup"><span data-stu-id="6c566-115">You can define a table as having hello IDENTITY property when you first create hello table by using syntax that is similar toohello following statement:</span></span>

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

<span data-ttu-id="6c566-116">Du kan sedan använda `INSERT..SELECT` toopopulate hello tabell.</span><span class="sxs-lookup"><span data-stu-id="6c566-116">You can then use `INSERT..SELECT` toopopulate hello table.</span></span>

## <a name="behavior"></a><span data-ttu-id="6c566-117">Beteende</span><span class="sxs-lookup"><span data-stu-id="6c566-117">Behavior</span></span>
<span data-ttu-id="6c566-118">hello IDENTITY-egenskapen är utformad tooscale ut över alla hello distributioner i hello data warehouse utan att läsa in prestanda påverkas.</span><span class="sxs-lookup"><span data-stu-id="6c566-118">hello IDENTITY property is designed tooscale out across all hello distributions in hello data warehouse without affecting load performance.</span></span> <span data-ttu-id="6c566-119">Därför är hello implementering av IDENTITETEN riktade mot att uppnå dessa mål.</span><span class="sxs-lookup"><span data-stu-id="6c566-119">Therefore, hello implementation of IDENTITY is oriented toward achieving these goals.</span></span> <span data-ttu-id="6c566-120">Det här avsnittet visar hello olika delarna av hello implementering toohelp du förstår dem. mer i detalj.</span><span class="sxs-lookup"><span data-stu-id="6c566-120">This section highlights hello nuances of hello implementation toohelp you understand them more fully.</span></span>  

### <a name="allocation-of-values"></a><span data-ttu-id="6c566-121">Allokering av värden</span><span class="sxs-lookup"><span data-stu-id="6c566-121">Allocation of values</span></span>
<span data-ttu-id="6c566-122">Hej identitetsegenskap garanterar inte hello-ordning i vilken hello allokeras surrogat värden, som visar hello beteendet för SQL Server och Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6c566-122">hello IDENTITY property doesn't guarantee hello order in which hello surrogate values are allocated, which reflects hello behavior of SQL Server and Azure SQL Database.</span></span> <span data-ttu-id="6c566-123">Men i Azure SQL Data Warehouse är hello frånvaron av en garanti tydligare.</span><span class="sxs-lookup"><span data-stu-id="6c566-123">However, in Azure SQL Data Warehouse, hello absence of a guarantee is more pronounced.</span></span> 

<span data-ttu-id="6c566-124">hello följande exempel är en bild:</span><span class="sxs-lookup"><span data-stu-id="6c566-124">hello following example is an illustration:</span></span>

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

<span data-ttu-id="6c566-125">I föregående exempel hello, landat två rader i distribution 1.</span><span class="sxs-lookup"><span data-stu-id="6c566-125">In hello preceding example, two rows landed in distribution 1.</span></span> <span data-ttu-id="6c566-126">hello första raden har hello surrogat värdet 1 i kolumnen `C1`, och hello andra raden har hello surrogat värde 61.</span><span class="sxs-lookup"><span data-stu-id="6c566-126">hello first row has hello surrogate value of 1 in column `C1`, and hello second row has hello surrogate value of 61.</span></span> <span data-ttu-id="6c566-127">Båda dessa värden har genererats av hello IDENTITY-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="6c566-127">Both of these values were generated by hello IDENTITY property.</span></span> <span data-ttu-id="6c566-128">Hello allokering av hello värden är inte sammanhängande.</span><span class="sxs-lookup"><span data-stu-id="6c566-128">However, hello allocation of hello values is not contiguous.</span></span> <span data-ttu-id="6c566-129">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="6c566-129">This behavior is by design.</span></span>

### <a name="skewed-data"></a><span data-ttu-id="6c566-130">Skeva data</span><span class="sxs-lookup"><span data-stu-id="6c566-130">Skewed data</span></span> 
<span data-ttu-id="6c566-131">hello värdeintervallet för hello datatyp jämnt fördelade över hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="6c566-131">hello range of values for hello data type are spread evenly across hello distributions.</span></span> <span data-ttu-id="6c566-132">Om en distribuerad tabell drabbas från skeva data, hello du värdeintervallet tillgängliga toohello datatype slut för tidigt.</span><span class="sxs-lookup"><span data-stu-id="6c566-132">If a distributed table suffers from skewed data, then hello range of values available toohello datatype can be exhausted prematurely.</span></span> <span data-ttu-id="6c566-133">Till exempel om alla hello data avslutas med en enda distributionsplats, sedan effektivt hello tabellen har åtkomst tooonly 1-/ 60 hello värden hello datatyp.</span><span class="sxs-lookup"><span data-stu-id="6c566-133">For example, if all hello data ends up in a single distribution, then effectively hello table has access tooonly one-sixtieth of hello values of hello data type.</span></span> <span data-ttu-id="6c566-134">Därför hello IDENTITY-egenskapen är begränsad för`INT` och `BIGINT` datatyperna endast.</span><span class="sxs-lookup"><span data-stu-id="6c566-134">For this reason, hello IDENTITY property is limited too`INT` and `BIGINT` data types only.</span></span>

### <a name="selectinto"></a><span data-ttu-id="6c566-135">VÄLJ... I</span><span class="sxs-lookup"><span data-stu-id="6c566-135">SELECT..INTO</span></span>
<span data-ttu-id="6c566-136">När en befintlig identitetskolumn väljs i en ny tabell, ärver hello ny kolumn hello IDENTITY-egenskapen, såvida inte någon av hello följande villkor är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="6c566-136">When an existing IDENTITY column is selected into a new table, hello new column inherits hello IDENTITY property, unless one of hello following conditions is true:</span></span>
- <span data-ttu-id="6c566-137">hello SELECT-satsen innehåller en koppling.</span><span class="sxs-lookup"><span data-stu-id="6c566-137">hello SELECT statement contains a join.</span></span>
- <span data-ttu-id="6c566-138">Flera SELECT-satser kopplas med hjälp av UNION.</span><span class="sxs-lookup"><span data-stu-id="6c566-138">Multiple SELECT statements are joined by using UNION.</span></span>
- <span data-ttu-id="6c566-139">hello identitetskolumnen visas mer än en gång i hello SELECT-listan.</span><span class="sxs-lookup"><span data-stu-id="6c566-139">hello IDENTITY column is listed more than one time in hello SELECT list.</span></span>
- <span data-ttu-id="6c566-140">hello IDENTITY-kolumn är en del av ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="6c566-140">hello IDENTITY column is part of an expression.</span></span>
    
<span data-ttu-id="6c566-141">Om någon av dessa villkor är SANT skapas hello kolumn inte är NULL i stället för att ärva hello IDENTITY-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="6c566-141">If any one of these conditions is true, hello column is created NOT NULL instead of inheriting hello IDENTITY property.</span></span>

### <a name="create-table-as-select"></a><span data-ttu-id="6c566-142">SKAPA TABLE AS SELECT</span><span class="sxs-lookup"><span data-stu-id="6c566-142">CREATE TABLE AS SELECT</span></span>
<span data-ttu-id="6c566-143">Skapa tabell AS Välj (CTAS) följer hello samma SQLServer-beteende som beskrivs i SELECT... I.</span><span class="sxs-lookup"><span data-stu-id="6c566-143">CREATE TABLE AS SELECT (CTAS) follows hello same SQL Server behavior that's documented for SELECT..INTO.</span></span> <span data-ttu-id="6c566-144">Men du kan inte ange en IDENTITY-egenskapen i hello kolumndefinition i hello `CREATE TABLE` tillhör hello-instruktion.</span><span class="sxs-lookup"><span data-stu-id="6c566-144">However, you can't specify an IDENTITY property in hello column definition of hello `CREATE TABLE` part of hello statement.</span></span> <span data-ttu-id="6c566-145">Du kan också använda hello IDENTITY-funktionen i hello `SELECT` tillhör hello CTAS.</span><span class="sxs-lookup"><span data-stu-id="6c566-145">You also can't use hello IDENTITY function in hello `SELECT` part of hello CTAS.</span></span> <span data-ttu-id="6c566-146">toopopulate en tabell, behöver du toouse `CREATE TABLE` toodefine hello tabell följt av `INSERT..SELECT` toopopulate den.</span><span class="sxs-lookup"><span data-stu-id="6c566-146">toopopulate a table, you need toouse `CREATE TABLE` toodefine hello table followed by `INSERT..SELECT` toopopulate it.</span></span>

## <a name="explicitly-insert-values-into-an-identity-column"></a><span data-ttu-id="6c566-147">Uttryckligen lägga till värden i en identitetskolumn</span><span class="sxs-lookup"><span data-stu-id="6c566-147">Explicitly insert values into an IDENTITY column</span></span> 
<span data-ttu-id="6c566-148">SQL Data Warehouse stöder `SET IDENTITY_INSERT <your table> ON|OFF` syntax.</span><span class="sxs-lookup"><span data-stu-id="6c566-148">SQL Data Warehouse supports `SET IDENTITY_INSERT <your table> ON|OFF` syntax.</span></span> <span data-ttu-id="6c566-149">Du kan använda den här syntaxen tooexplicitly infoga värden i hello identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="6c566-149">You can use this syntax tooexplicitly insert values into hello IDENTITY column.</span></span>

<span data-ttu-id="6c566-150">Många data Modellerare som toouse fördefinierade negativa värden för vissa rader i sina dimensioner.</span><span class="sxs-lookup"><span data-stu-id="6c566-150">Many data modelers like toouse predefined negative values for certain rows in their dimensions.</span></span> <span data-ttu-id="6c566-151">Ett exempel är hello -1 eller ”okänd medlem” rad.</span><span class="sxs-lookup"><span data-stu-id="6c566-151">An example is hello -1 or "unknown member" row.</span></span> 

<span data-ttu-id="6c566-152">hello nästa skript visar hur tooexplicitly lägger till den här raden med hjälp av ange IDENTITY_INSERT:</span><span class="sxs-lookup"><span data-stu-id="6c566-152">hello next script shows how tooexplicitly add this row by using SET IDENTITY_INSERT:</span></span>

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

## <a name="load-data-into-a-table-with-identity"></a><span data-ttu-id="6c566-153">Läs in data i en tabell med identitet</span><span class="sxs-lookup"><span data-stu-id="6c566-153">Load data into a table with IDENTITY</span></span>

<span data-ttu-id="6c566-154">hello förekomst av hello IDENTITY-egenskapen har effekter tooyour datainläsning kod.</span><span class="sxs-lookup"><span data-stu-id="6c566-154">hello presence of hello IDENTITY property has some implications tooyour data-loading code.</span></span> <span data-ttu-id="6c566-155">Det här avsnittet beskrivs vissa grundläggande mönster för att läsa in data i tabeller identiteten.</span><span class="sxs-lookup"><span data-stu-id="6c566-155">This section highlights some basic patterns for loading data into tables by using IDENTITY.</span></span> 

### <a name="load-data-with-polybase"></a><span data-ttu-id="6c566-156">Läs in data med PolyBase</span><span class="sxs-lookup"><span data-stu-id="6c566-156">Load data with PolyBase</span></span>
<span data-ttu-id="6c566-157">tooload data i en tabell och skapa surrogat nyckel identiteten, skapa hello tabell och sedan använda INSERT... Välj eller infoga... VÄRDEN tooperform hello belastning.</span><span class="sxs-lookup"><span data-stu-id="6c566-157">tooload data into a table and generate a surrogate key by using IDENTITY, create hello table and then use INSERT..SELECT or INSERT..VALUES tooperform hello load.</span></span>

<span data-ttu-id="6c566-158">hello visar följande exempel hello grundläggande mönster:</span><span class="sxs-lookup"><span data-stu-id="6c566-158">hello following example highlights hello basic pattern:</span></span>
 
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
> <span data-ttu-id="6c566-159">Det är inte möjligt toouse `CREATE TABLE AS SELECT` för närvarande vid inläsning av data i en tabell med en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="6c566-159">It's not possible toouse `CREATE TABLE AS SELECT` currently when loading data into a table with an IDENTITY column.</span></span>
> 

<span data-ttu-id="6c566-160">Mer information om att läsa in data med verktyget hello bulk copy program (BCP) finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="6c566-160">For more information on loading data by using hello bulk copy program (BCP) tool, see hello following articles:</span></span>

- <span data-ttu-id="6c566-161">[Läsa in med PolyBase][]</span><span class="sxs-lookup"><span data-stu-id="6c566-161">[Load with PolyBase][]</span></span>
- <span data-ttu-id="6c566-162">[PolyBase bästa praxis][]</span><span class="sxs-lookup"><span data-stu-id="6c566-162">[PolyBase best practices][]</span></span>

### <a name="load-data-with-bcp"></a><span data-ttu-id="6c566-163">Läs in data med BCP</span><span class="sxs-lookup"><span data-stu-id="6c566-163">Load data with BCP</span></span>
<span data-ttu-id="6c566-164">BCP är ett kommandoradsverktyg som du kan använda tooload data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6c566-164">BCP is a command-line tool that you can use tooload data into SQL Data Warehouse.</span></span> <span data-ttu-id="6c566-165">En av dess parametrar (-E) kontroller hello beteendet för BCP vid inläsning av data i en tabell med en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="6c566-165">One of its parameters (-E) controls hello behavior of BCP when loading data into a table with an IDENTITY column.</span></span> 

<span data-ttu-id="6c566-166">Om du har angett -E värde hello lagras i hello indatafilen för hello kolumn med IDENTITETEN.</span><span class="sxs-lookup"><span data-stu-id="6c566-166">When -E is specified, hello values held in hello input file for hello column with IDENTITY are retained.</span></span> <span data-ttu-id="6c566-167">Om E - *inte* anges ignoreras hello värden i den här kolumnen.</span><span class="sxs-lookup"><span data-stu-id="6c566-167">If -E is *not* specified, then hello values in this column are ignored.</span></span> <span data-ttu-id="6c566-168">Om hello identity-kolumn inte är inkluderad sedan hello data har lästs in som vanligt.</span><span class="sxs-lookup"><span data-stu-id="6c566-168">If hello identity column is not included, then hello data is loaded as normal.</span></span> <span data-ttu-id="6c566-169">hello värden genereras enligt toohello ökning och seed princip för hello-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="6c566-169">hello values are generated according toohello increment and seed policy of hello property.</span></span>

<span data-ttu-id="6c566-170">Mer information om att läsa in data med hjälp av BCP finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="6c566-170">For more information on loading data by using BCP, see hello following articles:</span></span>

- <span data-ttu-id="6c566-171">[Läsa in med BCP][]</span><span class="sxs-lookup"><span data-stu-id="6c566-171">[Load with BCP][]</span></span>
- <span data-ttu-id="6c566-172">[BCP i MSDN][]</span><span class="sxs-lookup"><span data-stu-id="6c566-172">[BCP in MSDN][]</span></span>

## <a name="catalog-views"></a><span data-ttu-id="6c566-173">Katalogvyer</span><span class="sxs-lookup"><span data-stu-id="6c566-173">Catalog views</span></span>
<span data-ttu-id="6c566-174">SQL Data Warehouse stöder hello `sys.identity_columns` vyn katalog.</span><span class="sxs-lookup"><span data-stu-id="6c566-174">SQL Data Warehouse supports hello `sys.identity_columns` catalog view.</span></span> <span data-ttu-id="6c566-175">Den här vyn kan vara används tooidentify en kolumn som innehåller hello IDENTITY-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="6c566-175">This view can be used tooidentify a column that has hello IDENTITY property.</span></span>

<span data-ttu-id="6c566-176">toohelp bättre förstå hello databasschemat, det här exemplet illustrerar hur toointegrate `sys.identity_columns` med andra system katalogvyer:</span><span class="sxs-lookup"><span data-stu-id="6c566-176">toohelp you better understand hello database schema, this example shows how toointegrate `sys.identity_columns` with other system catalog views:</span></span>

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

## <a name="limitations"></a><span data-ttu-id="6c566-177">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="6c566-177">Limitations</span></span>
<span data-ttu-id="6c566-178">hello IDENTITY-egenskapen kan inte användas i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="6c566-178">hello IDENTITY property can't be used in hello following scenarios:</span></span>
- <span data-ttu-id="6c566-179">Om hello Kolumndatatypen inte är INT eller BIGINT</span><span class="sxs-lookup"><span data-stu-id="6c566-179">Where hello column data type is not INT or BIGINT</span></span>
- <span data-ttu-id="6c566-180">Där hello kolumnen är också hello distribution</span><span class="sxs-lookup"><span data-stu-id="6c566-180">Where hello column is also hello distribution key</span></span>
- <span data-ttu-id="6c566-181">Där hello tabell är en extern tabell</span><span class="sxs-lookup"><span data-stu-id="6c566-181">Where hello table is an external table</span></span> 

<span data-ttu-id="6c566-182">hello följande relaterade funktioner stöds inte i SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="6c566-182">hello following related functions are not supported in SQL Data Warehouse:</span></span>

- <span data-ttu-id="6c566-183">[IDENTITY()][]</span><span class="sxs-lookup"><span data-stu-id="6c566-183">[IDENTITY()][]</span></span>
- <span data-ttu-id="6c566-184">[@@IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="6c566-184">[@@IDENTITY][]</span></span>
- <span data-ttu-id="6c566-185">[SCOPE_IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="6c566-185">[SCOPE_IDENTITY][]</span></span>
- <span data-ttu-id="6c566-186">[IDENT_CURRENT][]</span><span class="sxs-lookup"><span data-stu-id="6c566-186">[IDENT_CURRENT][]</span></span>
- <span data-ttu-id="6c566-187">[IDENT_INCR][]</span><span class="sxs-lookup"><span data-stu-id="6c566-187">[IDENT_INCR][]</span></span>
- <span data-ttu-id="6c566-188">[IDENT_SEED][]</span><span class="sxs-lookup"><span data-stu-id="6c566-188">[IDENT_SEED][]</span></span>
- <span data-ttu-id="6c566-189">[DBCC CHECK_IDENT()][]</span><span class="sxs-lookup"><span data-stu-id="6c566-189">[DBCC CHECK_IDENT()][]</span></span>

## <a name="tasks"></a><span data-ttu-id="6c566-190">Uppgifter</span><span class="sxs-lookup"><span data-stu-id="6c566-190">Tasks</span></span>

<span data-ttu-id="6c566-191">Det här avsnittet innehåller även kodexempel du kan använda tooperform vanliga uppgifter när du arbetar med identitetskolumner.</span><span class="sxs-lookup"><span data-stu-id="6c566-191">This section provides some sample code you can use tooperform common tasks when you work with IDENTITY columns.</span></span>

> [!NOTE] 
> <span data-ttu-id="6c566-192">Kolumnen C1 är hello identitet i alla hello följande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="6c566-192">Column C1 is hello IDENTITY in all hello following tasks.</span></span>
> 
 
### <a name="find-hello-highest-allocated-value-for-a-table"></a><span data-ttu-id="6c566-193">Hitta hello högsta allokerade värde för en tabell</span><span class="sxs-lookup"><span data-stu-id="6c566-193">Find hello highest allocated value for a table</span></span>
<span data-ttu-id="6c566-194">Använd hello `MAX()` fungerar toodetermine hello högsta värde som tilldelas för en distribuerad tabell:</span><span class="sxs-lookup"><span data-stu-id="6c566-194">Use hello `MAX()` function toodetermine hello highest value allocated for a distributed table:</span></span>

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-hello-seed-and-increment-for-hello-identity-property"></a><span data-ttu-id="6c566-195">Hitta hello start- och ökningsvärden för hello IDENTITY-egenskapen</span><span class="sxs-lookup"><span data-stu-id="6c566-195">Find hello seed and increment for hello IDENTITY property</span></span>
<span data-ttu-id="6c566-196">Du kan använda hello katalog vyer toodiscover hello identitet ökning och seed configuration värden för en tabell med hjälp av hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="6c566-196">You can use hello catalog views toodiscover hello identity increment and seed configuration values for a table by using hello following query:</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="6c566-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c566-197">Next steps</span></span>

* <span data-ttu-id="6c566-198">toolearn mer information om hur du utvecklar tabeller, se [tabell översikt][Overview], [tabell datatyper][Data Types], [distribuera en tabell] [ Distribute], [Index i en tabell][Index], [partitionera en tabell][Partition], och [ Temporära tabeller][Temporary].</span><span class="sxs-lookup"><span data-stu-id="6c566-198">toolearn more about developing tables, see [Table overview][Overview], [Table data types][Data Types], [Distribute a table][Distribute], [Index a table][Index], [Partition a table][Partition], and [Temporary tables][Temporary].</span></span> 
* <span data-ttu-id="6c566-199">Mer information om metodtips finns [SQL Data Warehouse metodtips][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="6c566-199">For more information about best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse Best Practices].</span></span>  

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
