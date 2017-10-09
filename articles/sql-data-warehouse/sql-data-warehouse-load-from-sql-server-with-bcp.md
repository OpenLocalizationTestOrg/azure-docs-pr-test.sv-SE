---
title: "aaaLoad data från SQL Server till Azure SQL Data Warehouse (bcp) | Microsoft Docs"
description: "För små datastorlekar, använder du bcp tooexport data från SQL Server tooflat filer och importera hello data direkt i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: a03b5403d123e8814ae73a7cce8e6851c8b264a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a><span data-ttu-id="3c491-103">Läs in data från SQL Server till Azure SQL Data Warehouse (flat-filer)</span><span class="sxs-lookup"><span data-stu-id="3c491-103">Load data from SQL Server into Azure SQL Data Warehouse (flat files)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c491-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="3c491-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="3c491-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="3c491-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="3c491-106">bcp</span><span class="sxs-lookup"><span data-stu-id="3c491-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="3c491-107">Du kan använda hello bcp kommandoradsverktyget tooexport data från SQL Server och läsa in den direkt tooAzure SQL Data Warehouse för små datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="3c491-107">For small data sets, you can use hello bcp command-line utility tooexport data from SQL Server and then load it directly tooAzure SQL Data Warehouse.</span></span>

<span data-ttu-id="3c491-108">I de här självstudierna kommer du att använda bcp för att:</span><span class="sxs-lookup"><span data-stu-id="3c491-108">In this tutorial, you will use bcp to:</span></span>

* <span data-ttu-id="3c491-109">Exportera en tabell från från SQL Server med hjälp av hello bcp out-kommandot (eller skapa en enkel exempelfil)</span><span class="sxs-lookup"><span data-stu-id="3c491-109">Export a table from from SQL Server by using hello bcp out command (or create a simple sample file)</span></span>
* <span data-ttu-id="3c491-110">Importera hello tabell från en flat fil tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3c491-110">Import hello table from a flat file tooSQL Data Warehouse.</span></span>
* <span data-ttu-id="3c491-111">Skapa statistik på hello läsa in data.</span><span class="sxs-lookup"><span data-stu-id="3c491-111">Create statistics on hello loaded data.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="3c491-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3c491-112">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="3c491-113">Krav</span><span class="sxs-lookup"><span data-stu-id="3c491-113">Prerequisites</span></span>
<span data-ttu-id="3c491-114">toostep igenom den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="3c491-114">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="3c491-115">En SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="3c491-115">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="3c491-116">hello kommandoradsverktyget BCP installerat</span><span class="sxs-lookup"><span data-stu-id="3c491-116">hello bcp command-line utility installed</span></span>
* <span data-ttu-id="3c491-117">hello kommandoradsverktyget sqlcmd installerat</span><span class="sxs-lookup"><span data-stu-id="3c491-117">hello sqlcmd command-line utility installed</span></span>

<span data-ttu-id="3c491-118">Du kan hämta verktygen bcp och sqlcmd för hello från hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="3c491-118">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="3c491-119">Data i ASCII- eller UTF-16-format</span><span class="sxs-lookup"><span data-stu-id="3c491-119">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="3c491-120">Om du provar den här självstudiekursen med dina egna data behöver data toouse hello ASCII- eller UTF-16-kodning eftersom bcp inte stöder UTF-8.</span><span class="sxs-lookup"><span data-stu-id="3c491-120">If you are trying this tutorial with your own data, your data needs toouse hello ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

<span data-ttu-id="3c491-121">PolyBase stöder UTF-8 men har inte än stöd för UTF-16.</span><span class="sxs-lookup"><span data-stu-id="3c491-121">PolyBase supports UTF-8 but doesn't yet support UTF-16.</span></span> <span data-ttu-id="3c491-122">Observera att om du vill toocombine bcp med PolyBase måste tootransform hello data tooUTF-8 efter att de exporterats från SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3c491-122">Note that if you want toocombine bcp with PolyBase you will need tootransform hello data tooUTF-8 after it is exported from SQL Server.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="3c491-123">1. Skapa en måltabell</span><span class="sxs-lookup"><span data-stu-id="3c491-123">1. Create a destination table</span></span>
<span data-ttu-id="3c491-124">Definiera en tabell i SQL Data Warehouse som kommer att hello måltabell för hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="3c491-124">Define a table in SQL Data Warehouse that will be hello destination table for hello load.</span></span> <span data-ttu-id="3c491-125">hello kolumner i tabellen hello måste överensstämma med toohello data i varje rad i datafilen.</span><span class="sxs-lookup"><span data-stu-id="3c491-125">hello columns in hello table must correspond toohello data in each row of your data file.</span></span>

<span data-ttu-id="3c491-126">toocreate en tabell, öppna en kommandotolk och använder sqlcmd.exe toorun hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3c491-126">toocreate a table, open a command prompt and use sqlcmd.exe toorun hello following command:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="3c491-127">2. Skapa en källdatafil</span><span class="sxs-lookup"><span data-stu-id="3c491-127">2. Create a source data file</span></span>
<span data-ttu-id="3c491-128">Öppna Anteckningar och kopiera hello följande rader med data i en ny textfil och sedan spara den här filen tooyour lokala temp-katalog, C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="3c491-128">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="3c491-129">Den här datan är i ASCII-format.</span><span class="sxs-lookup"><span data-stu-id="3c491-129">This data is in ASCII format.</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

<span data-ttu-id="3c491-130">(Valfritt) tooexport dina egna data från en SQL Server-databas, öppna en kommandotolk och kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="3c491-130">(Optional) tooexport your own data from a SQL Server database, open a command prompt and run hello following command.</span></span> <span data-ttu-id="3c491-131">Ersätt TableName, ServerName, DatabaseName, Username och Password med din egen information.</span><span class="sxs-lookup"><span data-stu-id="3c491-131">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-hello-data"></a><span data-ttu-id="3c491-132">3. Läs in hello data</span><span class="sxs-lookup"><span data-stu-id="3c491-132">3. Load hello data</span></span>
<span data-ttu-id="3c491-133">tooload hello data, öppna en kommandotolk och kör hello följande kommando, ersätter hello värdena för servernamn, databasen namn, användarnamn och lösenord med din egen information.</span><span class="sxs-lookup"><span data-stu-id="3c491-133">tooload hello data, open a command prompt and run hello following command, replacing hello values for Server Name, Database name, Username, and Password with your own information.</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="3c491-134">Använd det här kommandot tooverify hello data har lästs in korrekt</span><span class="sxs-lookup"><span data-stu-id="3c491-134">Use this command tooverify hello data was loaded properly</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="3c491-135">hello resultat bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="3c491-135">hello results should look like this:</span></span>

| <span data-ttu-id="3c491-136">DateId</span><span class="sxs-lookup"><span data-stu-id="3c491-136">DateId</span></span> | <span data-ttu-id="3c491-137">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="3c491-137">CalendarQuarter</span></span> | <span data-ttu-id="3c491-138">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="3c491-138">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3c491-139">20150101</span><span class="sxs-lookup"><span data-stu-id="3c491-139">20150101</span></span> |<span data-ttu-id="3c491-140">1</span><span class="sxs-lookup"><span data-stu-id="3c491-140">1</span></span> |<span data-ttu-id="3c491-141">3</span><span class="sxs-lookup"><span data-stu-id="3c491-141">3</span></span> |
| <span data-ttu-id="3c491-142">20150201</span><span class="sxs-lookup"><span data-stu-id="3c491-142">20150201</span></span> |<span data-ttu-id="3c491-143">1</span><span class="sxs-lookup"><span data-stu-id="3c491-143">1</span></span> |<span data-ttu-id="3c491-144">3</span><span class="sxs-lookup"><span data-stu-id="3c491-144">3</span></span> |
| <span data-ttu-id="3c491-145">20150301</span><span class="sxs-lookup"><span data-stu-id="3c491-145">20150301</span></span> |<span data-ttu-id="3c491-146">1</span><span class="sxs-lookup"><span data-stu-id="3c491-146">1</span></span> |<span data-ttu-id="3c491-147">3</span><span class="sxs-lookup"><span data-stu-id="3c491-147">3</span></span> |
| <span data-ttu-id="3c491-148">20150401</span><span class="sxs-lookup"><span data-stu-id="3c491-148">20150401</span></span> |<span data-ttu-id="3c491-149">2</span><span class="sxs-lookup"><span data-stu-id="3c491-149">2</span></span> |<span data-ttu-id="3c491-150">4</span><span class="sxs-lookup"><span data-stu-id="3c491-150">4</span></span> |
| <span data-ttu-id="3c491-151">20150501</span><span class="sxs-lookup"><span data-stu-id="3c491-151">20150501</span></span> |<span data-ttu-id="3c491-152">2</span><span class="sxs-lookup"><span data-stu-id="3c491-152">2</span></span> |<span data-ttu-id="3c491-153">4</span><span class="sxs-lookup"><span data-stu-id="3c491-153">4</span></span> |
| <span data-ttu-id="3c491-154">20150601</span><span class="sxs-lookup"><span data-stu-id="3c491-154">20150601</span></span> |<span data-ttu-id="3c491-155">2</span><span class="sxs-lookup"><span data-stu-id="3c491-155">2</span></span> |<span data-ttu-id="3c491-156">4</span><span class="sxs-lookup"><span data-stu-id="3c491-156">4</span></span> |
| <span data-ttu-id="3c491-157">20150701</span><span class="sxs-lookup"><span data-stu-id="3c491-157">20150701</span></span> |<span data-ttu-id="3c491-158">3</span><span class="sxs-lookup"><span data-stu-id="3c491-158">3</span></span> |<span data-ttu-id="3c491-159">1</span><span class="sxs-lookup"><span data-stu-id="3c491-159">1</span></span> |
| <span data-ttu-id="3c491-160">20150801</span><span class="sxs-lookup"><span data-stu-id="3c491-160">20150801</span></span> |<span data-ttu-id="3c491-161">3</span><span class="sxs-lookup"><span data-stu-id="3c491-161">3</span></span> |<span data-ttu-id="3c491-162">1</span><span class="sxs-lookup"><span data-stu-id="3c491-162">1</span></span> |
| <span data-ttu-id="3c491-163">20150801</span><span class="sxs-lookup"><span data-stu-id="3c491-163">20150801</span></span> |<span data-ttu-id="3c491-164">3</span><span class="sxs-lookup"><span data-stu-id="3c491-164">3</span></span> |<span data-ttu-id="3c491-165">1</span><span class="sxs-lookup"><span data-stu-id="3c491-165">1</span></span> |
| <span data-ttu-id="3c491-166">20151001</span><span class="sxs-lookup"><span data-stu-id="3c491-166">20151001</span></span> |<span data-ttu-id="3c491-167">4</span><span class="sxs-lookup"><span data-stu-id="3c491-167">4</span></span> |<span data-ttu-id="3c491-168">2</span><span class="sxs-lookup"><span data-stu-id="3c491-168">2</span></span> |
| <span data-ttu-id="3c491-169">20151101</span><span class="sxs-lookup"><span data-stu-id="3c491-169">20151101</span></span> |<span data-ttu-id="3c491-170">4</span><span class="sxs-lookup"><span data-stu-id="3c491-170">4</span></span> |<span data-ttu-id="3c491-171">2</span><span class="sxs-lookup"><span data-stu-id="3c491-171">2</span></span> |
| <span data-ttu-id="3c491-172">20151201</span><span class="sxs-lookup"><span data-stu-id="3c491-172">20151201</span></span> |<span data-ttu-id="3c491-173">4</span><span class="sxs-lookup"><span data-stu-id="3c491-173">4</span></span> |<span data-ttu-id="3c491-174">2</span><span class="sxs-lookup"><span data-stu-id="3c491-174">2</span></span> |

## <a name="4-create-statistics"></a><span data-ttu-id="3c491-175">4. Skapa statistik</span><span class="sxs-lookup"><span data-stu-id="3c491-175">4. Create statistics</span></span>
<span data-ttu-id="3c491-176">SQL Data Warehouse stöder inte automatiskt skapande eller uppdatering av statistik ännu.</span><span class="sxs-lookup"><span data-stu-id="3c491-176">SQL Data Warehouse does not yet support auto-create or auto-update statistics.</span></span> <span data-ttu-id="3c491-177">tooget hello bästa frågeprestanda, det är viktigt toocreate statistik på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data.</span><span class="sxs-lookup"><span data-stu-id="3c491-177">tooget hello best query performance, it's important toocreate statistics on all columns of all tables after hello first load or after any substantial changes occur in hello data.</span></span> <span data-ttu-id="3c491-178">En detaljerad förklaring av statistik finns [statistik][Statistics].</span><span class="sxs-lookup"><span data-stu-id="3c491-178">For a detailed explanation of statistics, see [Statistics][Statistics].</span></span> 

<span data-ttu-id="3c491-179">Kör följande kommando toocreate statistik på din nya inlästa tabell hello.</span><span class="sxs-lookup"><span data-stu-id="3c491-179">Run hello following command toocreate statistics on your newly loaded table.</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a><span data-ttu-id="3c491-180">5. Exportera data från SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3c491-180">5. Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="3c491-181">För skojs skull kan du exportera hello data som du just läste in, tillbaks ut ur SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3c491-181">For fun, you can export hello data that you just loaded back out of SQL Data Warehouse.</span></span>  <span data-ttu-id="3c491-182">hello kommandot tooexport är exakt hello samma som för att exportera från SQLServer.</span><span class="sxs-lookup"><span data-stu-id="3c491-182">hello command tooexport is exactly hello same as exporting from SQL Server.</span></span>

<span data-ttu-id="3c491-183">Det är dock något annorlunda hello resultat.</span><span class="sxs-lookup"><span data-stu-id="3c491-183">However, there is a difference in hello results.</span></span> <span data-ttu-id="3c491-184">Eftersom hello data lagras på distribuerade platser inom SQL Data Warehouse när du exporterar data skriver varje Compute-nod den data toohello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="3c491-184">Since hello data is stored in distributed locations within SQL Data Warehouse, when you export data each Compute node writes it data toohello output file.</span></span> <span data-ttu-id="3c491-185">hello är hello data i hello utdatafil sannolikt toobe annat än hello hello data i hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="3c491-185">hello order of hello data in hello output file is likely toobe different than hello order of hello data in hello input file.</span></span>

### <a name="export-a-table-and-compare-exported-results"></a><span data-ttu-id="3c491-186">Exportera en tabell och jämför exporterade resultat</span><span class="sxs-lookup"><span data-stu-id="3c491-186">Export a table and compare exported results</span></span>
<span data-ttu-id="3c491-187">toosee hello exporterade data, öppna en kommandotolk och kör det här kommandot med dina egna parametrar.</span><span class="sxs-lookup"><span data-stu-id="3c491-187">toosee hello exported data, open a command prompt and run this command using your own parameters.</span></span> <span data-ttu-id="3c491-188">ServerName är hello namnet på din logiska Azure SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3c491-188">ServerName is hello name of your Azure logical SQL Server.</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="3c491-189">Du kan kontrollera hello data exporterats korrekt genom att öppna nya hello-filen.</span><span class="sxs-lookup"><span data-stu-id="3c491-189">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="3c491-190">hello data i hello filen bör matcha hello texten nedan, men kommer antagligen vara sorterad i en annan ordning:</span><span class="sxs-lookup"><span data-stu-id="3c491-190">hello data in hello file should match hello text below, but will likely be sorted in a different order:</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-hello-results-of-a-query"></a><span data-ttu-id="3c491-191">Exportera hello resultatet av en fråga</span><span class="sxs-lookup"><span data-stu-id="3c491-191">Export hello results of a query</span></span>
<span data-ttu-id="3c491-192">Du kan använda hello **queryout** funktionen i bcp tooexport hello resultatet av en fråga istället för att exportera hello hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="3c491-192">You can use hello **queryout** function of bcp tooexport hello results of a query instead of exporting hello entire table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3c491-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c491-193">Next steps</span></span>
<span data-ttu-id="3c491-194">Se [Läs in data till SQL Data Warehouse][Load data into SQL Data Warehouse] för en översikt över inläsning.</span><span class="sxs-lookup"><span data-stu-id="3c491-194">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="3c491-195">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="3c491-195">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="3c491-196">Se [tabell översikt] [ Table Overview] eller [CREATE TABLE-syntax] [ CREATE TABLE syntax] för mer information om hur du skapar en tabell i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3c491-196">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse.</span></span>

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
