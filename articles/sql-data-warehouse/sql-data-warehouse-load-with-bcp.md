---
title: aaaUse bcp tooload data till SQL Data Warehouse | Microsoft Docs
description: "Lär dig vad bcp är och hur toouse det för informationslagerscenarier."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 09a2980585097644924c71899f9e74fb32fbc26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-bcp"></a><span data-ttu-id="c403e-103">Läsa in data med bcp</span><span class="sxs-lookup"><span data-stu-id="c403e-103">Load data with bcp</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c403e-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="c403e-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)  
> * [<span data-ttu-id="c403e-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="c403e-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [<span data-ttu-id="c403e-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="c403e-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [<span data-ttu-id="c403e-107">BCP</span><span class="sxs-lookup"><span data-stu-id="c403e-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="c403e-108">**[BCP] [ bcp]**  är ett kommandoradsverktyg för massinläsning som gör att du toocopy data mellan SQL Server, datafiler och SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c403e-108">**[bcp][bcp]** is a command-line bulk load utility that allows you toocopy data between SQL Server, data files, and SQL Data Warehouse.</span></span> <span data-ttu-id="c403e-109">Använd bcp tooimport stort antal rader till SQL Data Warehouse-tabeller eller tooexport data från SQL Server-tabeller till datafiler.</span><span class="sxs-lookup"><span data-stu-id="c403e-109">Use bcp tooimport large numbers of rows into SQL Data Warehouse tables or tooexport data from SQL Server tables into data files.</span></span> <span data-ttu-id="c403e-110">Förutom när det används med queryout-alternativet hello, behöver bcp inte Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="c403e-110">Except when used with hello queryout option, bcp requires no knowledge of Transact-SQL.</span></span>

<span data-ttu-id="c403e-111">BCP är ett snabbt och enkelt sätt toomove mindre datauppsättningar in och ut från en SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="c403e-111">bcp is a quick and easy way toomove smaller data sets into and out of a SQL Data Warehouse database.</span></span> <span data-ttu-id="c403e-112">hello exakta mängden data som tooload/extrahera med bcp rekommenderas beror på du nätverks anslutning toohello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="c403e-112">hello exact amount of data that is recommended tooload/extract via bcp will depend on you network connection toohello Azure data center.</span></span>  <span data-ttu-id="c403e-113">Generellt sett kan dimensionstabeller enkelt läsas in och extraheras med bcp, men bcp rekommenderas inte för inläsning eller extrahering av stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="c403e-113">Generally, dimension tables can be loaded and extracted readily with bcp, however, bcp is not recommended for loading or extracting large volumes of data.</span></span>  <span data-ttu-id="c403e-114">Polybase är hello rekommenderas för inläsning och extrahering av stora mängder data eftersom det är bättre utnyttja hello massivt parallella bearbetningsarkitekturen i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c403e-114">Polybase is hello recommended tool for loading and extracting large volumes of data as it does a better job leveraging hello massively parallel processing architecture of SQL Data Warehouse.</span></span>

<span data-ttu-id="c403e-115">Med bcp kan du:</span><span class="sxs-lookup"><span data-stu-id="c403e-115">With bcp you can:</span></span>

* <span data-ttu-id="c403e-116">Använd ett enkelt kommandoradsverktyg tooload data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c403e-116">Use a simple command-line utility tooload data into SQL Data Warehouse.</span></span>
* <span data-ttu-id="c403e-117">Använd ett enkelt kommandoradsverktyg tooextract data från SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c403e-117">Use a simple command-line utility tooextract data from SQL Data Warehouse.</span></span>

<span data-ttu-id="c403e-118">De här självstudierna visar hur du:</span><span class="sxs-lookup"><span data-stu-id="c403e-118">This tutorial will show you how to:</span></span>

* <span data-ttu-id="c403e-119">Importera data till en tabell med bcp hello i kommandot</span><span class="sxs-lookup"><span data-stu-id="c403e-119">Import data into a table using hello bcp in command</span></span>
* <span data-ttu-id="c403e-120">Exportera data från en tabell med hello bcp out-kommandot</span><span class="sxs-lookup"><span data-stu-id="c403e-120">Export data from a table uisng hello bcp out command</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="c403e-121">Krav</span><span class="sxs-lookup"><span data-stu-id="c403e-121">Prerequisites</span></span>
<span data-ttu-id="c403e-122">toostep igenom den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="c403e-122">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="c403e-123">En SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="c403e-123">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="c403e-124">hello kommandoradsverktyget bcp installerat</span><span class="sxs-lookup"><span data-stu-id="c403e-124">hello bcp command line utility installed</span></span>
* <span data-ttu-id="c403e-125">hello kommandoradsverktyget SQLCMD installerat</span><span class="sxs-lookup"><span data-stu-id="c403e-125">hello SQLCMD command line utility installed</span></span>

> [!NOTE]
> <span data-ttu-id="c403e-126">Du kan hämta verktygen bcp och sqlcmd för hello från hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="c403e-126">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a><span data-ttu-id="c403e-127">Importera data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c403e-127">Import data into SQL Data Warehouse</span></span>
<span data-ttu-id="c403e-128">I kursen får du skapa en tabell i Azure SQL Data Warehouse och importera data till hello tabell.</span><span class="sxs-lookup"><span data-stu-id="c403e-128">In this tutorial, you will create a table in Azure SQL Data Warehouse and import data into hello table.</span></span>

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a><span data-ttu-id="c403e-129">Steg 1: Skapa en tabell i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c403e-129">Step 1: Create a table in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="c403e-130">Använd sqlcmd toorun hello följande fråga toocreate en tabell på din instans från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="c403e-130">From a command prompt, use sqlcmd toorun hello following query toocreate a table on your instance:</span></span>

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
> <span data-ttu-id="c403e-131">Se [tabell översikt] [ Table Overview] eller [CREATE TABLE-syntax] [ CREATE TABLE syntax] för mer information om hur du skapar en tabell i SQL Data Warehouse och hello  alternativen i hello WITH-satsen.</span><span class="sxs-lookup"><span data-stu-id="c403e-131">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse and hello  options available in hello WITH clause.</span></span>
> 
> 

### <a name="step-2-create-a-source-data-file"></a><span data-ttu-id="c403e-132">Steg 2: Skapa en källdatafil</span><span class="sxs-lookup"><span data-stu-id="c403e-132">Step 2: Create a source data file</span></span>
<span data-ttu-id="c403e-133">Öppna Anteckningar och kopiera hello följande rader med data i en ny textfil och sedan spara den här filen tooyour lokala temp-katalog, C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="c403e-133">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span>

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
> <span data-ttu-id="c403e-134">Det är viktigt tooremember att bcp.exe inte har stöd för hello UTF-8 Filkodning.</span><span class="sxs-lookup"><span data-stu-id="c403e-134">It is important tooremember that bcp.exe does not support hello UTF-8 file encoding.</span></span> <span data-ttu-id="c403e-135">Använd ASCII-filer eller UTF-16-kodade filer när du använder bcp.exe.</span><span class="sxs-lookup"><span data-stu-id="c403e-135">Please use ASCII files or UTF-16 encoded files when using bcp.exe.</span></span>
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a><span data-ttu-id="c403e-136">Steg 3: Anslut och importera hello data</span><span class="sxs-lookup"><span data-stu-id="c403e-136">Step 3: Connect and import hello data</span></span>
<span data-ttu-id="c403e-137">Med bcp kan du ansluta och importera hello data med hjälp av följande kommando ersätter hello värdena med lämpliga sådana hello:</span><span class="sxs-lookup"><span data-stu-id="c403e-137">Using bcp, you can connect and import hello data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="c403e-138">Du kan verifiera hello data har lästs in genom att köra följande fråga med sqlcmd hello:</span><span class="sxs-lookup"><span data-stu-id="c403e-138">You can verify hello data was loaded by running hello following query using sqlcmd:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="c403e-139">Detta bör returnera hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="c403e-139">This should return hello following results:</span></span>

| <span data-ttu-id="c403e-140">DateId</span><span class="sxs-lookup"><span data-stu-id="c403e-140">DateId</span></span> | <span data-ttu-id="c403e-141">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="c403e-141">CalendarQuarter</span></span> | <span data-ttu-id="c403e-142">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="c403e-142">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c403e-143">20150101</span><span class="sxs-lookup"><span data-stu-id="c403e-143">20150101</span></span> |<span data-ttu-id="c403e-144">1</span><span class="sxs-lookup"><span data-stu-id="c403e-144">1</span></span> |<span data-ttu-id="c403e-145">3</span><span class="sxs-lookup"><span data-stu-id="c403e-145">3</span></span> |
| <span data-ttu-id="c403e-146">20150201</span><span class="sxs-lookup"><span data-stu-id="c403e-146">20150201</span></span> |<span data-ttu-id="c403e-147">1</span><span class="sxs-lookup"><span data-stu-id="c403e-147">1</span></span> |<span data-ttu-id="c403e-148">3</span><span class="sxs-lookup"><span data-stu-id="c403e-148">3</span></span> |
| <span data-ttu-id="c403e-149">20150301</span><span class="sxs-lookup"><span data-stu-id="c403e-149">20150301</span></span> |<span data-ttu-id="c403e-150">1</span><span class="sxs-lookup"><span data-stu-id="c403e-150">1</span></span> |<span data-ttu-id="c403e-151">3</span><span class="sxs-lookup"><span data-stu-id="c403e-151">3</span></span> |
| <span data-ttu-id="c403e-152">20150401</span><span class="sxs-lookup"><span data-stu-id="c403e-152">20150401</span></span> |<span data-ttu-id="c403e-153">2</span><span class="sxs-lookup"><span data-stu-id="c403e-153">2</span></span> |<span data-ttu-id="c403e-154">4</span><span class="sxs-lookup"><span data-stu-id="c403e-154">4</span></span> |
| <span data-ttu-id="c403e-155">20150501</span><span class="sxs-lookup"><span data-stu-id="c403e-155">20150501</span></span> |<span data-ttu-id="c403e-156">2</span><span class="sxs-lookup"><span data-stu-id="c403e-156">2</span></span> |<span data-ttu-id="c403e-157">4</span><span class="sxs-lookup"><span data-stu-id="c403e-157">4</span></span> |
| <span data-ttu-id="c403e-158">20150601</span><span class="sxs-lookup"><span data-stu-id="c403e-158">20150601</span></span> |<span data-ttu-id="c403e-159">2</span><span class="sxs-lookup"><span data-stu-id="c403e-159">2</span></span> |<span data-ttu-id="c403e-160">4</span><span class="sxs-lookup"><span data-stu-id="c403e-160">4</span></span> |
| <span data-ttu-id="c403e-161">20150701</span><span class="sxs-lookup"><span data-stu-id="c403e-161">20150701</span></span> |<span data-ttu-id="c403e-162">3</span><span class="sxs-lookup"><span data-stu-id="c403e-162">3</span></span> |<span data-ttu-id="c403e-163">1</span><span class="sxs-lookup"><span data-stu-id="c403e-163">1</span></span> |
| <span data-ttu-id="c403e-164">20150801</span><span class="sxs-lookup"><span data-stu-id="c403e-164">20150801</span></span> |<span data-ttu-id="c403e-165">3</span><span class="sxs-lookup"><span data-stu-id="c403e-165">3</span></span> |<span data-ttu-id="c403e-166">1</span><span class="sxs-lookup"><span data-stu-id="c403e-166">1</span></span> |
| <span data-ttu-id="c403e-167">20150801</span><span class="sxs-lookup"><span data-stu-id="c403e-167">20150801</span></span> |<span data-ttu-id="c403e-168">3</span><span class="sxs-lookup"><span data-stu-id="c403e-168">3</span></span> |<span data-ttu-id="c403e-169">1</span><span class="sxs-lookup"><span data-stu-id="c403e-169">1</span></span> |
| <span data-ttu-id="c403e-170">20151001</span><span class="sxs-lookup"><span data-stu-id="c403e-170">20151001</span></span> |<span data-ttu-id="c403e-171">4</span><span class="sxs-lookup"><span data-stu-id="c403e-171">4</span></span> |<span data-ttu-id="c403e-172">2</span><span class="sxs-lookup"><span data-stu-id="c403e-172">2</span></span> |
| <span data-ttu-id="c403e-173">20151101</span><span class="sxs-lookup"><span data-stu-id="c403e-173">20151101</span></span> |<span data-ttu-id="c403e-174">4</span><span class="sxs-lookup"><span data-stu-id="c403e-174">4</span></span> |<span data-ttu-id="c403e-175">2</span><span class="sxs-lookup"><span data-stu-id="c403e-175">2</span></span> |
| <span data-ttu-id="c403e-176">20151201</span><span class="sxs-lookup"><span data-stu-id="c403e-176">20151201</span></span> |<span data-ttu-id="c403e-177">4</span><span class="sxs-lookup"><span data-stu-id="c403e-177">4</span></span> |<span data-ttu-id="c403e-178">2</span><span class="sxs-lookup"><span data-stu-id="c403e-178">2</span></span> |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="c403e-179">Steg 4: Skapa statistik på dina nyinlästa data</span><span class="sxs-lookup"><span data-stu-id="c403e-179">Step 4: Create Statistics on your newly loaded data</span></span>
<span data-ttu-id="c403e-180">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="c403e-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span> <span data-ttu-id="c403e-181">I ordning tooget hello bästa prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data.</span><span class="sxs-lookup"><span data-stu-id="c403e-181">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span> <span data-ttu-id="c403e-182">En detaljerad förklaring av statistik finns hello [statistik] [ Statistics] -avsnittet i hello ämnesgruppen.</span><span class="sxs-lookup"><span data-stu-id="c403e-182">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span> <span data-ttu-id="c403e-183">Nedan visas ett enkelt exempel på hur toocreate statistik hello fram läses in i det här exemplet</span><span class="sxs-lookup"><span data-stu-id="c403e-183">Below is a quick example of how toocreate statistics on hello tabled loaded in this example</span></span>

<span data-ttu-id="c403e-184">Kör följande CREATE STATISTICS-uttryck från en sqlcmd-kommandotolk hello:</span><span class="sxs-lookup"><span data-stu-id="c403e-184">Execute hello following CREATE STATISTICS statements from a sqlcmd prompt:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a><span data-ttu-id="c403e-185">Exportera data från SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c403e-185">Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="c403e-186">I de här självstudierna kommer du att skapa en datafil från en tabell i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c403e-186">In this tutorial, you will create a data file from a table in SQL Data Warehouse.</span></span> <span data-ttu-id="c403e-187">Vi kommer att exportera hello data vi skapade ovan tooa ny datafil som heter DimDate2_export.txt.</span><span class="sxs-lookup"><span data-stu-id="c403e-187">We will export hello data we created above tooa new data file called DimDate2_export.txt.</span></span>

### <a name="step-1-export-hello-data"></a><span data-ttu-id="c403e-188">Steg 1: Exportera hello data</span><span class="sxs-lookup"><span data-stu-id="c403e-188">Step 1: Export hello data</span></span>
<span data-ttu-id="c403e-189">Använda hello bcp-verktyget kan du ansluta och exportera data med hjälp av följande kommando ersätter hello värdena med lämpliga sådana hello:</span><span class="sxs-lookup"><span data-stu-id="c403e-189">Using hello bcp utility, you can connect and export data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="c403e-190">Du kan kontrollera hello data exporterats korrekt genom att öppna nya hello-filen.</span><span class="sxs-lookup"><span data-stu-id="c403e-190">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="c403e-191">hello data i hello filen bör matcha hello texten nedan:</span><span class="sxs-lookup"><span data-stu-id="c403e-191">hello data in hello file should match hello text below:</span></span>

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
> <span data-ttu-id="c403e-192">På grund av toohello hur distribuerade system kanske inte hello dataordning hello samma över SQL Data Warehouse-databaser.</span><span class="sxs-lookup"><span data-stu-id="c403e-192">Due toohello nature of distributed systems, hello data order may not be hello same across SQL Data Warehouse databases.</span></span> <span data-ttu-id="c403e-193">Ett annat alternativ är toouse hello **queryout** funktionen i bcp toowrite en fråga extrakt istället exportera hello hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="c403e-193">Another option is toouse hello **queryout** function of bcp toowrite a query extract rather than export hello entire table.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="c403e-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c403e-194">Next steps</span></span>
<span data-ttu-id="c403e-195">Se [Läs in data till SQL Data Warehouse][Load data into SQL Data Warehouse] för en översikt över inläsning.</span><span class="sxs-lookup"><span data-stu-id="c403e-195">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="c403e-196">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="c403e-196">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
