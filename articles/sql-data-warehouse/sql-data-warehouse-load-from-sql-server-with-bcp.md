---
title: "Läs in data från SQL Server till Azure SQL Data Warehouse (bcp) | Microsoft Docs"
description: "För små datastorlekar, använder du bcp för att exportera data från SQL Server till flat-filer som sedan importeras direkt till Azure SQL Data Warehouse."
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
ms.openlocfilehash: dae7b5f7456f4ec0daf60d55f9c38b780896ff83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a><span data-ttu-id="adac5-103">Läs in data från SQL Server till Azure SQL Data Warehouse (flat-filer)</span><span class="sxs-lookup"><span data-stu-id="adac5-103">Load data from SQL Server into Azure SQL Data Warehouse (flat files)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="adac5-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="adac5-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="adac5-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="adac5-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="adac5-106">bcp</span><span class="sxs-lookup"><span data-stu-id="adac5-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="adac5-107">För små datauppsättningar, kan du använda kommandoradsverktyget bcp för att exportera data från SQL Server och läsa in den direkt i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="adac5-107">For small data sets, you can use the bcp command-line utility to export data from SQL Server and then load it directly to Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="adac5-108">I de här självstudierna kommer du att använda bcp för att:</span><span class="sxs-lookup"><span data-stu-id="adac5-108">In this tutorial, you will use bcp to:</span></span>

* <span data-ttu-id="adac5-109">Exportera en tabell från SQL Server med hjälp av bcp out-kommandot (eller genom att skapa en enkel exempelfil)</span><span class="sxs-lookup"><span data-stu-id="adac5-109">Export a table from from SQL Server by using the bcp out command (or create a simple sample file)</span></span>
* <span data-ttu-id="adac5-110">Importera tabellen från en flat-fil till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="adac5-110">Import the table from a flat file to SQL Data Warehouse.</span></span>
* <span data-ttu-id="adac5-111">Skapa statistik på inlästa data.</span><span class="sxs-lookup"><span data-stu-id="adac5-111">Create statistics on the loaded data.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="adac5-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="adac5-112">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="adac5-113">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="adac5-113">Prerequisites</span></span>
<span data-ttu-id="adac5-114">För att gå igenom de här självstudierna, behöver du:</span><span class="sxs-lookup"><span data-stu-id="adac5-114">To step through this tutorial, you need:</span></span>

* <span data-ttu-id="adac5-115">En SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="adac5-115">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="adac5-116">Kommandoradsverktyget bcp installerat</span><span class="sxs-lookup"><span data-stu-id="adac5-116">The bcp command-line utility installed</span></span>
* <span data-ttu-id="adac5-117">Kommandoradsverktyget sqlcmd installerat</span><span class="sxs-lookup"><span data-stu-id="adac5-117">The sqlcmd command-line utility installed</span></span>

<span data-ttu-id="adac5-118">Du kan hämta verktygen bcp och sqlcmd från [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="adac5-118">You can download the bcp and sqlcmd utilities from the [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="adac5-119">Data i ASCII- eller UTF-16-format</span><span class="sxs-lookup"><span data-stu-id="adac5-119">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="adac5-120">Om du använder egna data i självstudierna, måste de använda sig av ASCII- eller UTF-16-kodning eftersom bcp inte stöder UTF-8.</span><span class="sxs-lookup"><span data-stu-id="adac5-120">If you are trying this tutorial with your own data, your data needs to use the ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

<span data-ttu-id="adac5-121">PolyBase stöder UTF-8 men har inte än stöd för UTF-16.</span><span class="sxs-lookup"><span data-stu-id="adac5-121">PolyBase supports UTF-8 but doesn't yet support UTF-16.</span></span> <span data-ttu-id="adac5-122">Observera att om du vill kombinera bcp med PolyBase, behöver du transformera dina data till UTF-8 efter att de exporterats från SQL Server.</span><span class="sxs-lookup"><span data-stu-id="adac5-122">Note that if you want to combine bcp with PolyBase you will need to transform the data to UTF-8 after it is exported from SQL Server.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="adac5-123">1. Skapa en måltabell</span><span class="sxs-lookup"><span data-stu-id="adac5-123">1. Create a destination table</span></span>
<span data-ttu-id="adac5-124">Definiera en tabell i SQL Data Warehouse som kommer att vara måltabell för inläsningen.</span><span class="sxs-lookup"><span data-stu-id="adac5-124">Define a table in SQL Data Warehouse that will be the destination table for the load.</span></span> <span data-ttu-id="adac5-125">Kolumnerna i tabellen måste motsvara data i varje rad i din datafil.</span><span class="sxs-lookup"><span data-stu-id="adac5-125">The columns in the table must correspond to the data in each row of your data file.</span></span>

<span data-ttu-id="adac5-126">För att skapa en tabell, öppnar du en kommandotolk och använder sqlcmd.exe för att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="adac5-126">To create a table, open a command prompt and use sqlcmd.exe to run the following command:</span></span>

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


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="adac5-127">2. Skapa en källdatafil</span><span class="sxs-lookup"><span data-stu-id="adac5-127">2. Create a source data file</span></span>
<span data-ttu-id="adac5-128">Öppna Anteckningar och kopiera följande datarader till en ny textfil. Spara sedan filen till din lokala temp-katalog, C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="adac5-128">Open Notepad and copy the following lines of data into a new text file and then save this file to your local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="adac5-129">Den här datan är i ASCII-format.</span><span class="sxs-lookup"><span data-stu-id="adac5-129">This data is in ASCII format.</span></span>

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

<span data-ttu-id="adac5-130">(Valfritt) Om du vill exportera dina egna data från en SQL Server-databas, öppnar du en kommandotolk och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="adac5-130">(Optional) To export your own data from a SQL Server database, open a command prompt and run the following command.</span></span> <span data-ttu-id="adac5-131">Ersätt TableName, ServerName, DatabaseName, Username och Password med din egen information.</span><span class="sxs-lookup"><span data-stu-id="adac5-131">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a><span data-ttu-id="adac5-132">3. Läs in data</span><span class="sxs-lookup"><span data-stu-id="adac5-132">3. Load the data</span></span>
<span data-ttu-id="adac5-133">För att läsa in data, öppnar du en kommandotolk och kör följande kommando, där du ersätter värdena för servernamn, databasnamn, användarnamn och lösenord med din egen information.</span><span class="sxs-lookup"><span data-stu-id="adac5-133">To load the data, open a command prompt and run the following command, replacing the values for Server Name, Database name, Username, and Password with your own information.</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="adac5-134">Använd det här kommandot för att verifiera att data har lästs in korrekt</span><span class="sxs-lookup"><span data-stu-id="adac5-134">Use this command to verify the data was loaded properly</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="adac5-135">Resultatet borde se ut så här:</span><span class="sxs-lookup"><span data-stu-id="adac5-135">The results should look like this:</span></span>

| <span data-ttu-id="adac5-136">DateId</span><span class="sxs-lookup"><span data-stu-id="adac5-136">DateId</span></span> | <span data-ttu-id="adac5-137">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="adac5-137">CalendarQuarter</span></span> | <span data-ttu-id="adac5-138">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="adac5-138">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="adac5-139">20150101</span><span class="sxs-lookup"><span data-stu-id="adac5-139">20150101</span></span> |<span data-ttu-id="adac5-140">1</span><span class="sxs-lookup"><span data-stu-id="adac5-140">1</span></span> |<span data-ttu-id="adac5-141">3</span><span class="sxs-lookup"><span data-stu-id="adac5-141">3</span></span> |
| <span data-ttu-id="adac5-142">20150201</span><span class="sxs-lookup"><span data-stu-id="adac5-142">20150201</span></span> |<span data-ttu-id="adac5-143">1</span><span class="sxs-lookup"><span data-stu-id="adac5-143">1</span></span> |<span data-ttu-id="adac5-144">3</span><span class="sxs-lookup"><span data-stu-id="adac5-144">3</span></span> |
| <span data-ttu-id="adac5-145">20150301</span><span class="sxs-lookup"><span data-stu-id="adac5-145">20150301</span></span> |<span data-ttu-id="adac5-146">1</span><span class="sxs-lookup"><span data-stu-id="adac5-146">1</span></span> |<span data-ttu-id="adac5-147">3</span><span class="sxs-lookup"><span data-stu-id="adac5-147">3</span></span> |
| <span data-ttu-id="adac5-148">20150401</span><span class="sxs-lookup"><span data-stu-id="adac5-148">20150401</span></span> |<span data-ttu-id="adac5-149">2</span><span class="sxs-lookup"><span data-stu-id="adac5-149">2</span></span> |<span data-ttu-id="adac5-150">4</span><span class="sxs-lookup"><span data-stu-id="adac5-150">4</span></span> |
| <span data-ttu-id="adac5-151">20150501</span><span class="sxs-lookup"><span data-stu-id="adac5-151">20150501</span></span> |<span data-ttu-id="adac5-152">2</span><span class="sxs-lookup"><span data-stu-id="adac5-152">2</span></span> |<span data-ttu-id="adac5-153">4</span><span class="sxs-lookup"><span data-stu-id="adac5-153">4</span></span> |
| <span data-ttu-id="adac5-154">20150601</span><span class="sxs-lookup"><span data-stu-id="adac5-154">20150601</span></span> |<span data-ttu-id="adac5-155">2</span><span class="sxs-lookup"><span data-stu-id="adac5-155">2</span></span> |<span data-ttu-id="adac5-156">4</span><span class="sxs-lookup"><span data-stu-id="adac5-156">4</span></span> |
| <span data-ttu-id="adac5-157">20150701</span><span class="sxs-lookup"><span data-stu-id="adac5-157">20150701</span></span> |<span data-ttu-id="adac5-158">3</span><span class="sxs-lookup"><span data-stu-id="adac5-158">3</span></span> |<span data-ttu-id="adac5-159">1</span><span class="sxs-lookup"><span data-stu-id="adac5-159">1</span></span> |
| <span data-ttu-id="adac5-160">20150801</span><span class="sxs-lookup"><span data-stu-id="adac5-160">20150801</span></span> |<span data-ttu-id="adac5-161">3</span><span class="sxs-lookup"><span data-stu-id="adac5-161">3</span></span> |<span data-ttu-id="adac5-162">1</span><span class="sxs-lookup"><span data-stu-id="adac5-162">1</span></span> |
| <span data-ttu-id="adac5-163">20150801</span><span class="sxs-lookup"><span data-stu-id="adac5-163">20150801</span></span> |<span data-ttu-id="adac5-164">3</span><span class="sxs-lookup"><span data-stu-id="adac5-164">3</span></span> |<span data-ttu-id="adac5-165">1</span><span class="sxs-lookup"><span data-stu-id="adac5-165">1</span></span> |
| <span data-ttu-id="adac5-166">20151001</span><span class="sxs-lookup"><span data-stu-id="adac5-166">20151001</span></span> |<span data-ttu-id="adac5-167">4</span><span class="sxs-lookup"><span data-stu-id="adac5-167">4</span></span> |<span data-ttu-id="adac5-168">2</span><span class="sxs-lookup"><span data-stu-id="adac5-168">2</span></span> |
| <span data-ttu-id="adac5-169">20151101</span><span class="sxs-lookup"><span data-stu-id="adac5-169">20151101</span></span> |<span data-ttu-id="adac5-170">4</span><span class="sxs-lookup"><span data-stu-id="adac5-170">4</span></span> |<span data-ttu-id="adac5-171">2</span><span class="sxs-lookup"><span data-stu-id="adac5-171">2</span></span> |
| <span data-ttu-id="adac5-172">20151201</span><span class="sxs-lookup"><span data-stu-id="adac5-172">20151201</span></span> |<span data-ttu-id="adac5-173">4</span><span class="sxs-lookup"><span data-stu-id="adac5-173">4</span></span> |<span data-ttu-id="adac5-174">2</span><span class="sxs-lookup"><span data-stu-id="adac5-174">2</span></span> |

## <a name="4-create-statistics"></a><span data-ttu-id="adac5-175">4. Skapa statistik</span><span class="sxs-lookup"><span data-stu-id="adac5-175">4. Create statistics</span></span>
<span data-ttu-id="adac5-176">SQL Data Warehouse stöder inte automatiskt skapande eller uppdatering av statistik ännu.</span><span class="sxs-lookup"><span data-stu-id="adac5-176">SQL Data Warehouse does not yet support auto-create or auto-update statistics.</span></span> <span data-ttu-id="adac5-177">För att få bästa möjliga prestanda från dina frågor, är det viktigt att skapa statistik på alla kolumner i alla tabeller efter den första inläsningen eller vid betydande dataändringar.</span><span class="sxs-lookup"><span data-stu-id="adac5-177">To get the best query performance, it's important to create statistics on all columns of all tables after the first load or after any substantial changes occur in the data.</span></span> <span data-ttu-id="adac5-178">En detaljerad förklaring av statistik finns [statistik][Statistics].</span><span class="sxs-lookup"><span data-stu-id="adac5-178">For a detailed explanation of statistics, see [Statistics][Statistics].</span></span> 

<span data-ttu-id="adac5-179">Kör följande kommando för att skapa statistik på din nya inlästa tabell.</span><span class="sxs-lookup"><span data-stu-id="adac5-179">Run the following command to create statistics on your newly loaded table.</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a><span data-ttu-id="adac5-180">5. Exportera data från SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="adac5-180">5. Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="adac5-181">För skojs skull kan du exportera de data du just läste in, tillbaks ut ur SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="adac5-181">For fun, you can export the data that you just loaded back out of SQL Data Warehouse.</span></span>  <span data-ttu-id="adac5-182">Kommandot för att exportera är exakt detsamma som för att exportera från SQL Server.</span><span class="sxs-lookup"><span data-stu-id="adac5-182">The command to export is exactly the same as exporting from SQL Server.</span></span>

<span data-ttu-id="adac5-183">Resultatet är dock något annorlunda.</span><span class="sxs-lookup"><span data-stu-id="adac5-183">However, there is a difference in the results.</span></span> <span data-ttu-id="adac5-184">Eftersom data lagras på distribuerade platser inom SQL Data Warehouse så skriver varje Compute-nod sina data till utdatafilen när du exporterar data.</span><span class="sxs-lookup"><span data-stu-id="adac5-184">Since the data is stored in distributed locations within SQL Data Warehouse, when you export data each Compute node writes it data to the output file.</span></span> <span data-ttu-id="adac5-185">Dataordningen i utdatafilen skiljer sig med stor sannolikhet åt från dataordningen i indatafilen.</span><span class="sxs-lookup"><span data-stu-id="adac5-185">The order of the data in the output file is likely to be different than the order of the data in the input file.</span></span>

### <a name="export-a-table-and-compare-exported-results"></a><span data-ttu-id="adac5-186">Exportera en tabell och jämför exporterade resultat</span><span class="sxs-lookup"><span data-stu-id="adac5-186">Export a table and compare exported results</span></span>
<span data-ttu-id="adac5-187">Om du vill se exporterade data öppnar du en kommandotolk och kör det här kommandot med dina egna parametrar.</span><span class="sxs-lookup"><span data-stu-id="adac5-187">To see the exported data, open a command prompt and run this command using your own parameters.</span></span> <span data-ttu-id="adac5-188">ServerName är namnet på din logiska Azure SQL Server.</span><span class="sxs-lookup"><span data-stu-id="adac5-188">ServerName is the name of your Azure logical SQL Server.</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="adac5-189">Du kan verifiera att data exporterats korrekt genom att öppna den nya filen.</span><span class="sxs-lookup"><span data-stu-id="adac5-189">You can verify the data was exported correctly by opening the new file.</span></span> <span data-ttu-id="adac5-190">Data i filen bör matcha texten nedan, men kommer antagligen vara sorterad i en annan ordning:</span><span class="sxs-lookup"><span data-stu-id="adac5-190">The data in the file should match the text below, but will likely be sorted in a different order:</span></span>

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

### <a name="export-the-results-of-a-query"></a><span data-ttu-id="adac5-191">Exportera resultatet av en fråga</span><span class="sxs-lookup"><span data-stu-id="adac5-191">Export the results of a query</span></span>
<span data-ttu-id="adac5-192">Du kan använda bcp-funktionen **queryout** för att exportera resultaten av en fråga istället för att exportera hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="adac5-192">You can use the **queryout** function of bcp to export the results of a query instead of exporting the entire table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="adac5-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="adac5-193">Next steps</span></span>
<span data-ttu-id="adac5-194">Se [Läs in data till SQL Data Warehouse][Load data into SQL Data Warehouse] för en översikt över inläsning.</span><span class="sxs-lookup"><span data-stu-id="adac5-194">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="adac5-195">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="adac5-195">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="adac5-196">Se [tabell översikt] [ Table Overview] eller [CREATE TABLE-syntax] [ CREATE TABLE syntax] för mer information om hur du skapar en tabell i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="adac5-196">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse.</span></span>

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
