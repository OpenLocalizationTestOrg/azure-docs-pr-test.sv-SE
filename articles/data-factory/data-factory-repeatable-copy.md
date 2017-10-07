---
title: aaaRepeatable kopian i Azure Data Factory | Microsoft Docs
description: "Lär dig hur tooavoid duplicerar även om en sektor som kopierar data körs mer än en gång."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 79a3fde2b700bf0a0e167479d6a86c5bee1bf7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a><span data-ttu-id="61a12-103">Repeterbara kopian i Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="61a12-103">Repeatable copy in Azure Data Factory</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="61a12-104">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="61a12-104">Repeatable read from relational sources</span></span>
<span data-ttu-id="61a12-105">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="61a12-105">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="61a12-106">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="61a12-106">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="61a12-107">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="61a12-107">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="61a12-108">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="61a12-108">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span>  
 
> [!NOTE]
> <span data-ttu-id="61a12-109">hello följande exempel är för Azure SQL men tillämpliga tooany datalager som har stöd för rektangulär datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="61a12-109">hello following samples are for Azure SQL but are applicable tooany data store that supports rectangular datasets.</span></span> <span data-ttu-id="61a12-110">Du kan ha tooadjust hello **typen** för källan och hello **frågan** egenskapen (till exempel: frågan i stället sqlReaderQuery) hello data lagras.</span><span class="sxs-lookup"><span data-stu-id="61a12-110">You may have tooadjust hello **type** of source and hello **query** property (for example: query instead of sqlReaderQuery) for hello data store.</span></span>   

<span data-ttu-id="61a12-111">Vanligtvis vid läsning från relationella lagrar vill du att tooread endast hello data motsvarande toothat sektorn.</span><span class="sxs-lookup"><span data-stu-id="61a12-111">Usually, when reading from relational stores, you want tooread only hello data corresponding toothat slice.</span></span> <span data-ttu-id="61a12-112">Ett sätt toodo så är med hjälp av hello WindowStart och WindowEnd systemvariabler tillgängliga i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="61a12-112">A way toodo so would be by using hello WindowStart and WindowEnd system variables available in Azure Data Factory.</span></span> <span data-ttu-id="61a12-113">Läs mer om hello variabler och funktioner i Azure Data Factory här i hello [Azure Data Factory - funktioner och systemvariabler](data-factory-functions-variables.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="61a12-113">Read about hello variables and functions in Azure Data Factory here in hello [Azure Data Factory - Functions and System Variables](data-factory-functions-variables.md) article.</span></span> <span data-ttu-id="61a12-114">Exempel:</span><span class="sxs-lookup"><span data-stu-id="61a12-114">Example:</span></span> 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
<span data-ttu-id="61a12-115">Den här frågan läser data som finns i hello sektorn varaktighet intervallet (WindowStart -> WindowEnd) från hello tabellen mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="61a12-115">This query reads data that falls in hello slice duration range (WindowStart -> WindowEnd) from hello table MyTable.</span></span> <span data-ttu-id="61a12-116">Kör i det här segmentet skulle alltid se till att hello samma data läses.</span><span class="sxs-lookup"><span data-stu-id="61a12-116">Rerun of this slice would also always ensure that hello same data is read.</span></span> 

<span data-ttu-id="61a12-117">I annat fall vilja tooread hello hela tabellen och kan definiera hello sqlReaderQuery enligt följande:</span><span class="sxs-lookup"><span data-stu-id="61a12-117">In other cases, you may wish tooread hello entire table and may define hello sqlReaderQuery as follows:</span></span>

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-toosqlsink"></a><span data-ttu-id="61a12-118">Repeterbara skrivåtgärder tooSqlSink</span><span class="sxs-lookup"><span data-stu-id="61a12-118">Repeatable write tooSqlSink</span></span>
<span data-ttu-id="61a12-119">När du kopierar data för**Azure SQL/SQL Server** från andra datalager måste tookeep repeterbarhet i åtanke tooavoid oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="61a12-119">When copying data too**Azure SQL/SQL Server** from other data stores, you need tookeep repeatability in mind tooavoid unintended outcomes.</span></span> 

<span data-ttu-id="61a12-120">När du kopierar data tooAzure SQL/SQL Server-databasen läggs hello kopieringsaktiviteten datatabell toohello mottagare som standard.</span><span class="sxs-lookup"><span data-stu-id="61a12-120">When copying data tooAzure SQL/SQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="61a12-121">Säg, kopierar du data från en CSV-fil (fil med kommaavgränsade värden)-fil som innehåller två poster toohello i den följande tabellen i en Azure SQL/SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="61a12-121">Say, you are copying data from a CSV (comma-separated values) file containing two records toohello following table in an Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="61a12-122">När ett segment körs är hello två posterna kopierade toohello SQL-tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-122">When a slice runs, hello two records are copied toohello SQL table.</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="61a12-123">Du kanske fel har påträffats på källfilen och uppdatera hello antal ned röret från 2 too4.</span><span class="sxs-lookup"><span data-stu-id="61a12-123">Suppose you found errors in source file and updated hello quantity of Down Tube from 2 too4.</span></span> <span data-ttu-id="61a12-124">Om du manuellt köra hello datasektorn för denna period hittar du två nya poster läggs tooAzure SQL/SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="61a12-124">If you rerun hello data slice for that period manually, you’ll find two new records appended tooAzure SQL/SQL Server Database.</span></span> <span data-ttu-id="61a12-125">Det här exemplet förutsätter att ingen av hello kolumner i tabellen hello har hello primärnyckelns begränsning.</span><span class="sxs-lookup"><span data-stu-id="61a12-125">This example assumes that none of hello columns in hello table has hello primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="61a12-126">tooavoid problemet, du behöver toospecify UPSERT semantik med någon av följande två metoder hello:</span><span class="sxs-lookup"><span data-stu-id="61a12-126">tooavoid this behavior, you need toospecify UPSERT semantics by using one of hello following two mechanisms:</span></span>

### <a name="mechanism-1-using-sqlwritercleanupscript"></a><span data-ttu-id="61a12-127">Metod 1: använder sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="61a12-127">Mechanism 1: using sqlWriterCleanupScript</span></span>
<span data-ttu-id="61a12-128">Du kan använda hello **sqlWriterCleanupScript** egenskapen tooclean in data från hello sink tabell innan du infogar hello data när du kör ett segment.</span><span class="sxs-lookup"><span data-stu-id="61a12-128">You can use hello **sqlWriterCleanupScript** property tooclean up data from hello sink table before inserting hello data when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="61a12-129">När ett segment körs körs hello Rensa skript första toodelete data som motsvarar toohello segment från hello SQL-tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-129">When a slice runs, hello cleanup script is run first toodelete data that corresponds toohello slice from hello SQL table.</span></span> <span data-ttu-id="61a12-130">Hej kopieringsaktiviteten infogar data i hello SQL-tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-130">hello copy activity then inserts data into hello SQL Table.</span></span> <span data-ttu-id="61a12-131">Om hello sektorn körs hello kvantitet uppdateras enligt önskemål.</span><span class="sxs-lookup"><span data-stu-id="61a12-131">If hello slice is rerun, hello quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="61a12-132">Anta att hello Flat bricka post tas bort från hello ursprungliga csv.</span><span class="sxs-lookup"><span data-stu-id="61a12-132">Suppose hello Flat Washer record is removed from hello original csv.</span></span> <span data-ttu-id="61a12-133">Köra hello sektorn skulle sedan skapa hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="61a12-133">Then rerunning hello slice would produce hello following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="61a12-134">hello kopieringsaktiviteten kördes hello Rensa skriptet toodelete hello motsvarande data för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="61a12-134">hello copy activity ran hello cleanup script toodelete hello corresponding data for that slice.</span></span> <span data-ttu-id="61a12-135">Sedan den läsa hello indata från hello csv (som sedan finns bara en post) och infogas i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-135">Then it read hello input from hello csv (which then contained only one record) and inserted it into hello Table.</span></span> 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a><span data-ttu-id="61a12-136">Metod 2: använda sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="61a12-136">Mechanism 2: using sliceIdentifierColumnName</span></span>
> [!IMPORTANT]
> <span data-ttu-id="61a12-137">SliceIdentifierColumnName stöds för närvarande inte för Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="61a12-137">Currently, sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="61a12-138">hello andra mekanism tooachieve repeterbarhet är genom att ha en dedikerad kolumn (sliceIdentifierColumnName) i hello mål tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-138">hello second mechanism tooachieve repeatability is by having a dedicated column (sliceIdentifierColumnName) in hello target Table.</span></span> <span data-ttu-id="61a12-139">Den här kolumnen kan användas av Azure Data Factory tooensure hello käll- och fortsätter att synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="61a12-139">This column would be used by Azure Data Factory tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="61a12-140">Den här metoden fungerar när det finns flexibilitet för att ändra eller definiera hello mål SQL tabellens schema.</span><span class="sxs-lookup"><span data-stu-id="61a12-140">This approach works when there is flexibility in changing or defining hello destination SQL Table schema.</span></span> 

<span data-ttu-id="61a12-141">Den här kolumnen används av Azure Data Factory för repeterbarhet och pågående hello Azure Data Factory inte göra några schemat ändras toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-141">This column is used by Azure Data Factory for repeatability purposes and in hello process Azure Data Factory does not make any schema changes toohello Table.</span></span> <span data-ttu-id="61a12-142">Sätt toouse den här metoden:</span><span class="sxs-lookup"><span data-stu-id="61a12-142">Way toouse this approach:</span></span>

1. <span data-ttu-id="61a12-143">Definiera en kolumn av typen **binary (32)** i hello målet SQL-tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-143">Define a column of type **binary (32)** in hello destination SQL Table.</span></span> <span data-ttu-id="61a12-144">Det bör finnas några begränsningar på den här kolumnen.</span><span class="sxs-lookup"><span data-stu-id="61a12-144">There should be no constraints on this column.</span></span> <span data-ttu-id="61a12-145">Vi namn på den här kolumnen som AdfSliceIdentifier för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="61a12-145">Let's name this column as AdfSliceIdentifier for this example.</span></span>


    <span data-ttu-id="61a12-146">Källtabellen:</span><span class="sxs-lookup"><span data-stu-id="61a12-146">Source table:</span></span>

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    <span data-ttu-id="61a12-147">Måltabellen:</span><span class="sxs-lookup"><span data-stu-id="61a12-147">Destination table:</span></span> 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. <span data-ttu-id="61a12-148">Använd den i hello kopieringsaktiviteten enligt följande:</span><span class="sxs-lookup"><span data-stu-id="61a12-148">Use it in hello copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

<span data-ttu-id="61a12-149">Azure Data Factory fylls den här kolumnen enligt dess måste tooensure hello käll- och hålls synkroniserade.</span><span class="sxs-lookup"><span data-stu-id="61a12-149">Azure Data Factory populates this column as per its need tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="61a12-150">hello värdena i den här kolumnen ska inte användas utanför den här kontexten.</span><span class="sxs-lookup"><span data-stu-id="61a12-150">hello values of this column should not be used outside of this context.</span></span> 

<span data-ttu-id="61a12-151">Liknande toomechanism 1, Kopieringsaktiviteten rensas automatiskt hello data för hello angivna segment från hello målet SQL-tabell.</span><span class="sxs-lookup"><span data-stu-id="61a12-151">Similar toomechanism 1, Copy Activity automatically cleans up hello data for hello given slice from hello destination SQL Table.</span></span> <span data-ttu-id="61a12-152">Därefter infogas data från källan i toohello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="61a12-152">It then inserts data from source in toohello destination table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="61a12-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61a12-153">Next steps</span></span>
<span data-ttu-id="61a12-154">Granska hello efter anslutningen artiklar som slutförts JSON-exempel:</span><span class="sxs-lookup"><span data-stu-id="61a12-154">Review hello following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="61a12-155">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="61a12-155">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="61a12-156">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="61a12-156">Azure SQL Data Warehouse</span></span>](data-factory-azure-sql-data-warehouse-connector.md)
- [<span data-ttu-id="61a12-157">SQL Server</span><span class="sxs-lookup"><span data-stu-id="61a12-157">SQL Server</span></span>](data-factory-sqlserver-connector.md)