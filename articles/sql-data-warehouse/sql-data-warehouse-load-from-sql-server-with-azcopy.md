---
title: "Läs in data från SQL Server till Azure SQL Data Warehouse (PolyBase) | Microsoft Docs"
description: "Använder sig av bcp för att exportera data från SQL Server till flat-filer, AZCopy för att importera data till Azure blobblagring och PolyBase för att mata in data i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 08f94bc26d8b1e1f5a56dfccd41e394dbd721024
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a><span data-ttu-id="71e7d-103">Läs in data från SQL Server till Azure SQL Data Warehouse (AZCopy)</span><span class="sxs-lookup"><span data-stu-id="71e7d-103">Load data from SQL Server into Azure SQL Data Warehouse (AZCopy)</span></span>
<span data-ttu-id="71e7d-104">Använd kommandoradsverktygen bcp och AZCopy för att läsa in data från SQL Server till Azure blobblagring.</span><span class="sxs-lookup"><span data-stu-id="71e7d-104">Use bcp and AZCopy command-line utilities to load data from SQL Server to Azure blob storage.</span></span> <span data-ttu-id="71e7d-105">Använd sedan PolyBase eller Azure Data Factory för att läsa in data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="71e7d-105">Then use PolyBase or Azure Data Factory to load the data into Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="71e7d-106">Krav</span><span class="sxs-lookup"><span data-stu-id="71e7d-106">Prerequisites</span></span>
<span data-ttu-id="71e7d-107">För att gå igenom de här självstudierna, behöver du:</span><span class="sxs-lookup"><span data-stu-id="71e7d-107">To step through this tutorial, you need:</span></span>

* <span data-ttu-id="71e7d-108">En SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="71e7d-108">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="71e7d-109">Kommandoradsverktyget bcp installerat</span><span class="sxs-lookup"><span data-stu-id="71e7d-109">The bcp command line utility installed</span></span>
* <span data-ttu-id="71e7d-110">Kommandoradsverktyget SQLCMD installerat</span><span class="sxs-lookup"><span data-stu-id="71e7d-110">The SQLCMD command line utility installed</span></span>

> [!NOTE]
> <span data-ttu-id="71e7d-111">Du kan hämta verktygen bcp och sqlcmd från [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="71e7d-111">You can download the bcp and sqlcmd utilities from the [Microsoft Download Center][Microsoft Download Center].</span></span>
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a><span data-ttu-id="71e7d-112">Importera data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="71e7d-112">Import data into SQL Data Warehouse</span></span>
<span data-ttu-id="71e7d-113">I de här självstudierna kommer du att skapa en tabell i Azure SQL Data Warehouse och importera data till tabellen.</span><span class="sxs-lookup"><span data-stu-id="71e7d-113">In this tutorial, you will create a table in Azure SQL Data Warehouse and import data into the table.</span></span>

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a><span data-ttu-id="71e7d-114">Steg 1: Skapa en tabell i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="71e7d-114">Step 1: Create a table in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="71e7d-115">Använd sqlcmd från en kommandotolk för att köra följande fråga och skapa en tabell på din instans:</span><span class="sxs-lookup"><span data-stu-id="71e7d-115">From a command prompt, use sqlcmd to run the following query to create a table on your instance:</span></span>

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

> [!NOTE]
> <span data-ttu-id="71e7d-116">Se [Tabellöversikt][Table Overview] eller [CREATE TABLE-syntax][CREATE TABLE syntax] för ytterligare information om hur man skapar en tabell i SQL Data Warehouse och alternativen som finns i WITH-satsen.</span><span class="sxs-lookup"><span data-stu-id="71e7d-116">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse and the  options available in the WITH clause.</span></span>
> 
> 

### <a name="step-2-create-a-source-data-file"></a><span data-ttu-id="71e7d-117">Steg 2: Skapa en källdatafil</span><span class="sxs-lookup"><span data-stu-id="71e7d-117">Step 2: Create a source data file</span></span>
<span data-ttu-id="71e7d-118">Öppna Anteckningar och kopiera följande datarader till en ny textfil. Spara sedan filen till din lokala temp-katalog, C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="71e7d-118">Open Notepad and copy the following lines of data into a new text file and then save this file to your local temp directory, C:\Temp\DimDate2.txt.</span></span>

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

> [!NOTE]
> <span data-ttu-id="71e7d-119">Det är viktigt att komma ihåg att bcp.exe inte har stöd för UTF-8 filkodning.</span><span class="sxs-lookup"><span data-stu-id="71e7d-119">It is important to remember that bcp.exe does not support the UTF-8 file encoding.</span></span> <span data-ttu-id="71e7d-120">Använd ASCII-filer eller UTF-16-kodade filer när du använder bcp.exe.</span><span class="sxs-lookup"><span data-stu-id="71e7d-120">Please use ASCII files or UTF-16 encoded files when using bcp.exe.</span></span>
> 
> 

### <a name="step-3-connect-and-import-the-data"></a><span data-ttu-id="71e7d-121">Steg 3: Anslut och importera data</span><span class="sxs-lookup"><span data-stu-id="71e7d-121">Step 3: Connect and import the data</span></span>
<span data-ttu-id="71e7d-122">Med bcp kan du ansluta och importera data med hjälp av följande kommando, där du ersätter värdena med lämpliga sådana:</span><span class="sxs-lookup"><span data-stu-id="71e7d-122">Using bcp, you can connect and import the data using the following command replacing the values as appropriate:</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="71e7d-123">Du kan verifiera att data lästs in genom att köra följande fråga med hjälp av sqlcmd:</span><span class="sxs-lookup"><span data-stu-id="71e7d-123">You can verify the data was loaded by running the following query using sqlcmd:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="71e7d-124">Det ska returnera följande resultat:</span><span class="sxs-lookup"><span data-stu-id="71e7d-124">This should return the following results:</span></span>

| <span data-ttu-id="71e7d-125">DateId</span><span class="sxs-lookup"><span data-stu-id="71e7d-125">DateId</span></span> | <span data-ttu-id="71e7d-126">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="71e7d-126">CalendarQuarter</span></span> | <span data-ttu-id="71e7d-127">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="71e7d-127">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="71e7d-128">20150101</span><span class="sxs-lookup"><span data-stu-id="71e7d-128">20150101</span></span> |<span data-ttu-id="71e7d-129">1</span><span class="sxs-lookup"><span data-stu-id="71e7d-129">1</span></span> |<span data-ttu-id="71e7d-130">3</span><span class="sxs-lookup"><span data-stu-id="71e7d-130">3</span></span> |
| <span data-ttu-id="71e7d-131">20150201</span><span class="sxs-lookup"><span data-stu-id="71e7d-131">20150201</span></span> |<span data-ttu-id="71e7d-132">1</span><span class="sxs-lookup"><span data-stu-id="71e7d-132">1</span></span> |<span data-ttu-id="71e7d-133">3</span><span class="sxs-lookup"><span data-stu-id="71e7d-133">3</span></span> |
| <span data-ttu-id="71e7d-134">20150301</span><span class="sxs-lookup"><span data-stu-id="71e7d-134">20150301</span></span> |<span data-ttu-id="71e7d-135">1</span><span class="sxs-lookup"><span data-stu-id="71e7d-135">1</span></span> |<span data-ttu-id="71e7d-136">3</span><span class="sxs-lookup"><span data-stu-id="71e7d-136">3</span></span> |
| <span data-ttu-id="71e7d-137">20150401</span><span class="sxs-lookup"><span data-stu-id="71e7d-137">20150401</span></span> |<span data-ttu-id="71e7d-138">2</span><span class="sxs-lookup"><span data-stu-id="71e7d-138">2</span></span> |<span data-ttu-id="71e7d-139">4</span><span class="sxs-lookup"><span data-stu-id="71e7d-139">4</span></span> |
| <span data-ttu-id="71e7d-140">20150501</span><span class="sxs-lookup"><span data-stu-id="71e7d-140">20150501</span></span> |<span data-ttu-id="71e7d-141">2</span><span class="sxs-lookup"><span data-stu-id="71e7d-141">2</span></span> |<span data-ttu-id="71e7d-142">4</span><span class="sxs-lookup"><span data-stu-id="71e7d-142">4</span></span> |
| <span data-ttu-id="71e7d-143">20150601</span><span class="sxs-lookup"><span data-stu-id="71e7d-143">20150601</span></span> |<span data-ttu-id="71e7d-144">2</span><span class="sxs-lookup"><span data-stu-id="71e7d-144">2</span></span> |<span data-ttu-id="71e7d-145">4</span><span class="sxs-lookup"><span data-stu-id="71e7d-145">4</span></span> |
| <span data-ttu-id="71e7d-146">20150701</span><span class="sxs-lookup"><span data-stu-id="71e7d-146">20150701</span></span> |<span data-ttu-id="71e7d-147">3</span><span class="sxs-lookup"><span data-stu-id="71e7d-147">3</span></span> |<span data-ttu-id="71e7d-148">1</span><span class="sxs-lookup"><span data-stu-id="71e7d-148">1</span></span> |
| <span data-ttu-id="71e7d-149">20150801</span><span class="sxs-lookup"><span data-stu-id="71e7d-149">20150801</span></span> |<span data-ttu-id="71e7d-150">3</span><span class="sxs-lookup"><span data-stu-id="71e7d-150">3</span></span> |<span data-ttu-id="71e7d-151">1</span><span class="sxs-lookup"><span data-stu-id="71e7d-151">1</span></span> |
| <span data-ttu-id="71e7d-152">20150801</span><span class="sxs-lookup"><span data-stu-id="71e7d-152">20150801</span></span> |<span data-ttu-id="71e7d-153">3</span><span class="sxs-lookup"><span data-stu-id="71e7d-153">3</span></span> |<span data-ttu-id="71e7d-154">1</span><span class="sxs-lookup"><span data-stu-id="71e7d-154">1</span></span> |
| <span data-ttu-id="71e7d-155">20151001</span><span class="sxs-lookup"><span data-stu-id="71e7d-155">20151001</span></span> |<span data-ttu-id="71e7d-156">4</span><span class="sxs-lookup"><span data-stu-id="71e7d-156">4</span></span> |<span data-ttu-id="71e7d-157">2</span><span class="sxs-lookup"><span data-stu-id="71e7d-157">2</span></span> |
| <span data-ttu-id="71e7d-158">20151101</span><span class="sxs-lookup"><span data-stu-id="71e7d-158">20151101</span></span> |<span data-ttu-id="71e7d-159">4</span><span class="sxs-lookup"><span data-stu-id="71e7d-159">4</span></span> |<span data-ttu-id="71e7d-160">2</span><span class="sxs-lookup"><span data-stu-id="71e7d-160">2</span></span> |
| <span data-ttu-id="71e7d-161">20151201</span><span class="sxs-lookup"><span data-stu-id="71e7d-161">20151201</span></span> |<span data-ttu-id="71e7d-162">4</span><span class="sxs-lookup"><span data-stu-id="71e7d-162">4</span></span> |<span data-ttu-id="71e7d-163">2</span><span class="sxs-lookup"><span data-stu-id="71e7d-163">2</span></span> |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="71e7d-164">Steg 4: Skapa statistik på dina nyinlästa data</span><span class="sxs-lookup"><span data-stu-id="71e7d-164">Step 4: Create Statistics on your newly loaded data</span></span>
<span data-ttu-id="71e7d-165">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="71e7d-165">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span> <span data-ttu-id="71e7d-166">För att få bästa möjliga prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter den första inläsningen eller vid betydande dataändringar.</span><span class="sxs-lookup"><span data-stu-id="71e7d-166">In order to get the best performance from your queries, it's important that statistics be created on all columns of all tables after the first load or any substantial changes occur in the data.</span></span> <span data-ttu-id="71e7d-167">En detaljerad förklaring av statistik finns i ämnet [Statistik][Statistics] i ämnesgruppen Utveckla.</span><span class="sxs-lookup"><span data-stu-id="71e7d-167">For a detailed explanation of statistics, see the [Statistics][Statistics] topic in the Develop group of topics.</span></span> <span data-ttu-id="71e7d-168">Nedan följer ett enkelt exempel på hur du skapar statistik på tabellerna som lästs in i det här exemplet</span><span class="sxs-lookup"><span data-stu-id="71e7d-168">Below is a quick example of how to create statistics on the tabled loaded in this example</span></span>

<span data-ttu-id="71e7d-169">Kör följande CREATE STATISTICS-uttryck från en sqlcmd-kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="71e7d-169">Execute the following CREATE STATISTICS statements from a sqlcmd prompt:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a><span data-ttu-id="71e7d-170">Exportera data från SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="71e7d-170">Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="71e7d-171">I de här självstudierna kommer du att skapa en datafil från en tabell i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="71e7d-171">In this tutorial, you will create a data file from a table in SQL Data Warehouse.</span></span> <span data-ttu-id="71e7d-172">Vi kommer att exportera de data vi skapade ovan till en ny datafil som heter DimDate2_export.txt.</span><span class="sxs-lookup"><span data-stu-id="71e7d-172">We will export the data we created above to a new data file called DimDate2_export.txt.</span></span>

### <a name="step-1-export-the-data"></a><span data-ttu-id="71e7d-173">Steg 1: Exportera data</span><span class="sxs-lookup"><span data-stu-id="71e7d-173">Step 1: Export the data</span></span>
<span data-ttu-id="71e7d-174">Med bcp-verktyget kan du ansluta och exportera data med hjälp av följande kommando, där du ersätter värdena med lämpliga sådana:</span><span class="sxs-lookup"><span data-stu-id="71e7d-174">Using the bcp utility, you can connect and export data using the following command replacing the values as appropriate:</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="71e7d-175">Du kan verifiera att data exporterats korrekt genom att öppna den nya filen.</span><span class="sxs-lookup"><span data-stu-id="71e7d-175">You can verify the data was exported correctly by opening the new file.</span></span> <span data-ttu-id="71e7d-176">De data som finns i filen ska matcha texten nedan:</span><span class="sxs-lookup"><span data-stu-id="71e7d-176">The data in the file should match the text below:</span></span>

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

> [!NOTE]
> <span data-ttu-id="71e7d-177">På grund av hur distribuerade system fungerar, är det möjligt att dataordningen inte är samma i alla SQL Data Warehouse-databaser.</span><span class="sxs-lookup"><span data-stu-id="71e7d-177">Due to the nature of distributed systems, the data order may not be the same across SQL Data Warehouse databases.</span></span> <span data-ttu-id="71e7d-178">Ett annat alternativ är att använda **queryout**-funktionen i bcp för att skriva ett fråge-extrakt istället för att exportera hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="71e7d-178">Another option is to use the **queryout** function of bcp to write a query extract rather than export the entire table.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="71e7d-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71e7d-179">Next steps</span></span>
<span data-ttu-id="71e7d-180">Se [Läs in data till SQL Data Warehouse][Load data into SQL Data Warehouse] för en översikt över inläsning.</span><span class="sxs-lookup"><span data-stu-id="71e7d-180">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="71e7d-181">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="71e7d-181">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
