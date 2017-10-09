---
title: "aaaLoad data från SQL Server till Azure SQL Data Warehouse (PolyBase) | Microsoft Docs"
description: "Använder bcp tooexport data från SQL Server tooflat filer, AZCopy tooimport data tooAzure blobblagring och PolyBase tooingest hello data till Azure SQL Data Warehouse."
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
ms.openlocfilehash: 89e2a91bc97642e9fc18545cb802b42d8dc4ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a><span data-ttu-id="67983-103">Läs in data från SQL Server till Azure SQL Data Warehouse (AZCopy)</span><span class="sxs-lookup"><span data-stu-id="67983-103">Load data from SQL Server into Azure SQL Data Warehouse (AZCopy)</span></span>
<span data-ttu-id="67983-104">Använd bcp och AZCopy kommandoradsverktyg tooload data från SQL Server tooAzure blob storage.</span><span class="sxs-lookup"><span data-stu-id="67983-104">Use bcp and AZCopy command-line utilities tooload data from SQL Server tooAzure blob storage.</span></span> <span data-ttu-id="67983-105">Använd sedan PolyBase eller Azure Data Factory tooload hello data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="67983-105">Then use PolyBase or Azure Data Factory tooload hello data into Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="67983-106">Krav</span><span class="sxs-lookup"><span data-stu-id="67983-106">Prerequisites</span></span>
<span data-ttu-id="67983-107">toostep igenom den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="67983-107">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="67983-108">En SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="67983-108">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="67983-109">hello kommandoradsverktyget bcp installerat</span><span class="sxs-lookup"><span data-stu-id="67983-109">hello bcp command line utility installed</span></span>
* <span data-ttu-id="67983-110">hello kommandoradsverktyget SQLCMD installerat</span><span class="sxs-lookup"><span data-stu-id="67983-110">hello SQLCMD command line utility installed</span></span>

> [!NOTE]
> <span data-ttu-id="67983-111">Du kan hämta verktygen bcp och sqlcmd för hello från hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="67983-111">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a><span data-ttu-id="67983-112">Importera data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="67983-112">Import data into SQL Data Warehouse</span></span>
<span data-ttu-id="67983-113">I kursen får du skapa en tabell i Azure SQL Data Warehouse och importera data till hello tabell.</span><span class="sxs-lookup"><span data-stu-id="67983-113">In this tutorial, you will create a table in Azure SQL Data Warehouse and import data into hello table.</span></span>

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a><span data-ttu-id="67983-114">Steg 1: Skapa en tabell i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="67983-114">Step 1: Create a table in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="67983-115">Använd sqlcmd toorun hello följande fråga toocreate en tabell på din instans från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="67983-115">From a command prompt, use sqlcmd toorun hello following query toocreate a table on your instance:</span></span>

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
> <span data-ttu-id="67983-116">Se [tabell översikt] [ Table Overview] eller [CREATE TABLE-syntax] [ CREATE TABLE syntax] för mer information om hur du skapar en tabell i SQL Data Warehouse och hello  alternativen i hello WITH-satsen.</span><span class="sxs-lookup"><span data-stu-id="67983-116">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse and hello  options available in hello WITH clause.</span></span>
> 
> 

### <a name="step-2-create-a-source-data-file"></a><span data-ttu-id="67983-117">Steg 2: Skapa en källdatafil</span><span class="sxs-lookup"><span data-stu-id="67983-117">Step 2: Create a source data file</span></span>
<span data-ttu-id="67983-118">Öppna Anteckningar och kopiera hello följande rader med data i en ny textfil och sedan spara den här filen tooyour lokala temp-katalog, C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="67983-118">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span>

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
> <span data-ttu-id="67983-119">Det är viktigt tooremember att bcp.exe inte har stöd för hello UTF-8 Filkodning.</span><span class="sxs-lookup"><span data-stu-id="67983-119">It is important tooremember that bcp.exe does not support hello UTF-8 file encoding.</span></span> <span data-ttu-id="67983-120">Använd ASCII-filer eller UTF-16-kodade filer när du använder bcp.exe.</span><span class="sxs-lookup"><span data-stu-id="67983-120">Please use ASCII files or UTF-16 encoded files when using bcp.exe.</span></span>
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a><span data-ttu-id="67983-121">Steg 3: Anslut och importera hello data</span><span class="sxs-lookup"><span data-stu-id="67983-121">Step 3: Connect and import hello data</span></span>
<span data-ttu-id="67983-122">Med bcp kan du ansluta och importera hello data med hjälp av följande kommando ersätter hello värdena med lämpliga sådana hello:</span><span class="sxs-lookup"><span data-stu-id="67983-122">Using bcp, you can connect and import hello data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="67983-123">Du kan verifiera hello data har lästs in genom att köra följande fråga med sqlcmd hello:</span><span class="sxs-lookup"><span data-stu-id="67983-123">You can verify hello data was loaded by running hello following query using sqlcmd:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="67983-124">Detta bör returnera hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="67983-124">This should return hello following results:</span></span>

| <span data-ttu-id="67983-125">DateId</span><span class="sxs-lookup"><span data-stu-id="67983-125">DateId</span></span> | <span data-ttu-id="67983-126">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="67983-126">CalendarQuarter</span></span> | <span data-ttu-id="67983-127">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="67983-127">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="67983-128">20150101</span><span class="sxs-lookup"><span data-stu-id="67983-128">20150101</span></span> |<span data-ttu-id="67983-129">1</span><span class="sxs-lookup"><span data-stu-id="67983-129">1</span></span> |<span data-ttu-id="67983-130">3</span><span class="sxs-lookup"><span data-stu-id="67983-130">3</span></span> |
| <span data-ttu-id="67983-131">20150201</span><span class="sxs-lookup"><span data-stu-id="67983-131">20150201</span></span> |<span data-ttu-id="67983-132">1</span><span class="sxs-lookup"><span data-stu-id="67983-132">1</span></span> |<span data-ttu-id="67983-133">3</span><span class="sxs-lookup"><span data-stu-id="67983-133">3</span></span> |
| <span data-ttu-id="67983-134">20150301</span><span class="sxs-lookup"><span data-stu-id="67983-134">20150301</span></span> |<span data-ttu-id="67983-135">1</span><span class="sxs-lookup"><span data-stu-id="67983-135">1</span></span> |<span data-ttu-id="67983-136">3</span><span class="sxs-lookup"><span data-stu-id="67983-136">3</span></span> |
| <span data-ttu-id="67983-137">20150401</span><span class="sxs-lookup"><span data-stu-id="67983-137">20150401</span></span> |<span data-ttu-id="67983-138">2</span><span class="sxs-lookup"><span data-stu-id="67983-138">2</span></span> |<span data-ttu-id="67983-139">4</span><span class="sxs-lookup"><span data-stu-id="67983-139">4</span></span> |
| <span data-ttu-id="67983-140">20150501</span><span class="sxs-lookup"><span data-stu-id="67983-140">20150501</span></span> |<span data-ttu-id="67983-141">2</span><span class="sxs-lookup"><span data-stu-id="67983-141">2</span></span> |<span data-ttu-id="67983-142">4</span><span class="sxs-lookup"><span data-stu-id="67983-142">4</span></span> |
| <span data-ttu-id="67983-143">20150601</span><span class="sxs-lookup"><span data-stu-id="67983-143">20150601</span></span> |<span data-ttu-id="67983-144">2</span><span class="sxs-lookup"><span data-stu-id="67983-144">2</span></span> |<span data-ttu-id="67983-145">4</span><span class="sxs-lookup"><span data-stu-id="67983-145">4</span></span> |
| <span data-ttu-id="67983-146">20150701</span><span class="sxs-lookup"><span data-stu-id="67983-146">20150701</span></span> |<span data-ttu-id="67983-147">3</span><span class="sxs-lookup"><span data-stu-id="67983-147">3</span></span> |<span data-ttu-id="67983-148">1</span><span class="sxs-lookup"><span data-stu-id="67983-148">1</span></span> |
| <span data-ttu-id="67983-149">20150801</span><span class="sxs-lookup"><span data-stu-id="67983-149">20150801</span></span> |<span data-ttu-id="67983-150">3</span><span class="sxs-lookup"><span data-stu-id="67983-150">3</span></span> |<span data-ttu-id="67983-151">1</span><span class="sxs-lookup"><span data-stu-id="67983-151">1</span></span> |
| <span data-ttu-id="67983-152">20150801</span><span class="sxs-lookup"><span data-stu-id="67983-152">20150801</span></span> |<span data-ttu-id="67983-153">3</span><span class="sxs-lookup"><span data-stu-id="67983-153">3</span></span> |<span data-ttu-id="67983-154">1</span><span class="sxs-lookup"><span data-stu-id="67983-154">1</span></span> |
| <span data-ttu-id="67983-155">20151001</span><span class="sxs-lookup"><span data-stu-id="67983-155">20151001</span></span> |<span data-ttu-id="67983-156">4</span><span class="sxs-lookup"><span data-stu-id="67983-156">4</span></span> |<span data-ttu-id="67983-157">2</span><span class="sxs-lookup"><span data-stu-id="67983-157">2</span></span> |
| <span data-ttu-id="67983-158">20151101</span><span class="sxs-lookup"><span data-stu-id="67983-158">20151101</span></span> |<span data-ttu-id="67983-159">4</span><span class="sxs-lookup"><span data-stu-id="67983-159">4</span></span> |<span data-ttu-id="67983-160">2</span><span class="sxs-lookup"><span data-stu-id="67983-160">2</span></span> |
| <span data-ttu-id="67983-161">20151201</span><span class="sxs-lookup"><span data-stu-id="67983-161">20151201</span></span> |<span data-ttu-id="67983-162">4</span><span class="sxs-lookup"><span data-stu-id="67983-162">4</span></span> |<span data-ttu-id="67983-163">2</span><span class="sxs-lookup"><span data-stu-id="67983-163">2</span></span> |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="67983-164">Steg 4: Skapa statistik på dina nyinlästa data</span><span class="sxs-lookup"><span data-stu-id="67983-164">Step 4: Create Statistics on your newly loaded data</span></span>
<span data-ttu-id="67983-165">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="67983-165">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span> <span data-ttu-id="67983-166">I ordning tooget hello bästa prestanda från dina frågor, är det viktigt att statistik skapas på alla kolumner i alla tabeller efter hello första inläsningen eller vid betydande dataändringar i hello data.</span><span class="sxs-lookup"><span data-stu-id="67983-166">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span> <span data-ttu-id="67983-167">En detaljerad förklaring av statistik finns hello [statistik] [ Statistics] -avsnittet i hello ämnesgruppen.</span><span class="sxs-lookup"><span data-stu-id="67983-167">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span> <span data-ttu-id="67983-168">Nedan visas ett enkelt exempel på hur toocreate statistik hello fram läses in i det här exemplet</span><span class="sxs-lookup"><span data-stu-id="67983-168">Below is a quick example of how toocreate statistics on hello tabled loaded in this example</span></span>

<span data-ttu-id="67983-169">Kör följande CREATE STATISTICS-uttryck från en sqlcmd-kommandotolk hello:</span><span class="sxs-lookup"><span data-stu-id="67983-169">Execute hello following CREATE STATISTICS statements from a sqlcmd prompt:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a><span data-ttu-id="67983-170">Exportera data från SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="67983-170">Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="67983-171">I de här självstudierna kommer du att skapa en datafil från en tabell i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="67983-171">In this tutorial, you will create a data file from a table in SQL Data Warehouse.</span></span> <span data-ttu-id="67983-172">Vi kommer att exportera hello data vi skapade ovan tooa ny datafil som heter DimDate2_export.txt.</span><span class="sxs-lookup"><span data-stu-id="67983-172">We will export hello data we created above tooa new data file called DimDate2_export.txt.</span></span>

### <a name="step-1-export-hello-data"></a><span data-ttu-id="67983-173">Steg 1: Exportera hello data</span><span class="sxs-lookup"><span data-stu-id="67983-173">Step 1: Export hello data</span></span>
<span data-ttu-id="67983-174">Använda hello bcp-verktyget kan du ansluta och exportera data med hjälp av följande kommando ersätter hello värdena med lämpliga sådana hello:</span><span class="sxs-lookup"><span data-stu-id="67983-174">Using hello bcp utility, you can connect and export data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="67983-175">Du kan kontrollera hello data exporterats korrekt genom att öppna nya hello-filen.</span><span class="sxs-lookup"><span data-stu-id="67983-175">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="67983-176">hello data i hello filen bör matcha hello texten nedan:</span><span class="sxs-lookup"><span data-stu-id="67983-176">hello data in hello file should match hello text below:</span></span>

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
> <span data-ttu-id="67983-177">På grund av toohello hur distribuerade system kanske inte hello dataordning hello samma över SQL Data Warehouse-databaser.</span><span class="sxs-lookup"><span data-stu-id="67983-177">Due toohello nature of distributed systems, hello data order may not be hello same across SQL Data Warehouse databases.</span></span> <span data-ttu-id="67983-178">Ett annat alternativ är toouse hello **queryout** funktionen i bcp toowrite en fråga extrakt istället exportera hello hela tabellen.</span><span class="sxs-lookup"><span data-stu-id="67983-178">Another option is toouse hello **queryout** function of bcp toowrite a query extract rather than export hello entire table.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="67983-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67983-179">Next steps</span></span>
<span data-ttu-id="67983-180">Se [Läs in data till SQL Data Warehouse][Load data into SQL Data Warehouse] för en översikt över inläsning.</span><span class="sxs-lookup"><span data-stu-id="67983-180">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="67983-181">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="67983-181">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
