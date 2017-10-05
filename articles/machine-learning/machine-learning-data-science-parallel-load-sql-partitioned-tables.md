---
title: "Skapa och optimera tabeller för snabb parallella import av data till en SQL Server på en virtuell dator i Azure | Microsoft Docs"
description: "Parallell massimport av data med SQL-tabeller för partition"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ff90fdb0-5bc7-49e8-aee7-678b54f901c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: aae4e4f59e76bf48b00a2ee92aedd7d5643ba91a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a><span data-ttu-id="94a14-103">Parallell massimport av data med SQL-tabeller för partition</span><span class="sxs-lookup"><span data-stu-id="94a14-103">Parallel Bulk Data Import Using SQL Partition Tables</span></span>
<span data-ttu-id="94a14-104">Det här dokumentet beskrivs hur du skapar partitionerade tabeller för snabb parallella massimport av data till en SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="94a14-104">This document describes how to build partitioned tables for fast parallel bulk importing of data to a SQL Server database.</span></span> <span data-ttu-id="94a14-105">För stora inläsning/överföring av data till en SQL-databas, importera data till SQL-databas och efterföljande frågor kan förbättras genom att använda *partitionerade tabeller och vyer*.</span><span class="sxs-lookup"><span data-stu-id="94a14-105">For big data loading/transfer to a SQL database, importing data to the SQL DB and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a><span data-ttu-id="94a14-106">Skapa en ny databas och en uppsättning filgrupper</span><span class="sxs-lookup"><span data-stu-id="94a14-106">Create a new database and a set of filegroups</span></span>
* <span data-ttu-id="94a14-107">[Skapa en ny databas](https://technet.microsoft.com/library/ms176061.aspx), om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="94a14-107">[Create a new database](https://technet.microsoft.com/library/ms176061.aspx), if it doesn't exist already.</span></span>
* <span data-ttu-id="94a14-108">Lägga till databasen filgrupper i databasen som innehåller de partitionerade fysiska filerna. Detta kan göras med [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) om nya eller [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) om databasen finns redan.</span><span class="sxs-lookup"><span data-stu-id="94a14-108">Add database filegroups to the database which will hold the partitioned physical files.This can be done with [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) if new or [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) if the database exists already.</span></span>
* <span data-ttu-id="94a14-109">Lägga till en eller flera filer (om det behövs) i varje filgrupp för databasen.</span><span class="sxs-lookup"><span data-stu-id="94a14-109">Add one or more files (as needed) to each database filegroup.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="94a14-110">Ange mål-filgrupp som innehåller data för den här partitionen och den fysiska databasen filnamn där filgruppsdata i kommer att sparas.</span><span class="sxs-lookup"><span data-stu-id="94a14-110">Specify the target filegroup which holds data for this partition and the physical database file name(s) where the filegroup data will be stored.</span></span>
  > 
  > 

<span data-ttu-id="94a14-111">I följande exempel skapas en ny databas med tre filgrupper än primärt och loggrupper, som innehåller en fysisk fil i varje.</span><span class="sxs-lookup"><span data-stu-id="94a14-111">The following example creates a new database with three filegroups other than the primary and log groups, containing one physical file in each.</span></span> <span data-ttu-id="94a14-112">Databasfilerna skapas i SQL Server-Data standardmapp som konfigurerats i SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="94a14-112">The database files are created in the default SQL Server Data folder, as configured in the SQL Server instance.</span></span> <span data-ttu-id="94a14-113">Läs mer om standardsökvägar [filplatser för standard- och namngivna instanser av SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).</span><span class="sxs-lookup"><span data-stu-id="94a14-113">For more information about the default file locations, see [File Locations for Default and Named Instances of SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).</span></span>

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

    EXECUTE ('
        CREATE DATABASE <database_name>
         ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a><span data-ttu-id="94a14-114">Skapa en partitionerad tabell</span><span class="sxs-lookup"><span data-stu-id="94a14-114">Create a partitioned table</span></span>
<span data-ttu-id="94a14-115">Skapa partitionerade tabeller enligt dataschemat mappas till databasen filgrupperna skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="94a14-115">Create partitioned table(s) according to the data schema, mapped to the database filegroups created in the previous step.</span></span> <span data-ttu-id="94a14-116">När data samtidigt som importeras i en partitionerad tabell fördelas posterna mellan filgrupper enligt ett partitionsschema som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="94a14-116">When data is bulk imported to the partitioned table(s), records will be distributed among the filegroups according to a partition scheme, as described below.</span></span>

<span data-ttu-id="94a14-117">**Om du vill skapa en partitionstabell, måste du:**</span><span class="sxs-lookup"><span data-stu-id="94a14-117">**To create a partition table, you need to:**</span></span>

* <span data-ttu-id="94a14-118">[Skapa en partitionsfunktion](https://msdn.microsoft.com/library/ms187802.aspx) som definierar en uppsättning värden/gränser som ska ingå i varje enskild partitionstabell, t.ex., för att begränsa partitioner per månad (vissa\_datetime\_fältet) år 2013:</span><span class="sxs-lookup"><span data-stu-id="94a14-118">[Create a partition function](https://msdn.microsoft.com/library/ms187802.aspx) which defines the range of values/boundaries to be included in each individual partition table, e.g., to limit partitions by month(some\_datetime\_field) in the year 2013:</span></span>
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )
* <span data-ttu-id="94a14-119">[Skapa ett partitionsschema](https://msdn.microsoft.com/library/ms179854.aspx) som mappar varje partition intervallet i partitionsfunktionen till en fysisk filgrupp, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="94a14-119">[Create a partition scheme](https://msdn.microsoft.com/library/ms179854.aspx) which maps each partition range in the partition function to a physical filegroup, e.g.:</span></span>
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  <span data-ttu-id="94a14-120">Kontrollera intervall gäller i varje partition enligt funktionen/schemat genom att köra följande fråga:</span><span class="sxs-lookup"><span data-stu-id="94a14-120">To verify the ranges in effect in each partition according to the function/scheme, run the following query:</span></span>
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>
* <span data-ttu-id="94a14-121">[Skapa en partitionerad tabell](https://msdn.microsoft.com/library/ms174979.aspx)(s) enligt dataschemat och ange partition schema och begränsning fält som används för att partitionera tabellen, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="94a14-121">[Create partitioned table](https://msdn.microsoft.com/library/ms174979.aspx)(s) according to your data schema, and specify the partition scheme and constraint field used to partition the table, e.g.:</span></span>
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

<span data-ttu-id="94a14-122">Mer information finns i [skapa partitionerade tabeller och index](https://msdn.microsoft.com/library/ms188730.aspx).</span><span class="sxs-lookup"><span data-stu-id="94a14-122">For more information, see [Create Partitioned Tables and Indexes](https://msdn.microsoft.com/library/ms188730.aspx).</span></span>

## <a name="bulk-import-the-data-for-each-individual-partition-table"></a><span data-ttu-id="94a14-123">Massimportera data för varje enskild partitionstabellen</span><span class="sxs-lookup"><span data-stu-id="94a14-123">Bulk import the data for each individual partition table</span></span>
* <span data-ttu-id="94a14-124">Du kan använda BCP BULK INSERT eller andra metoder som [Migreringsguiden för SQL Server](http://sqlazuremw.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="94a14-124">You may use BCP, BULK INSERT, or other methods such as [SQL Server Migration Wizard](http://sqlazuremw.codeplex.com/).</span></span> <span data-ttu-id="94a14-125">Följande exempel används metoden BCP.</span><span class="sxs-lookup"><span data-stu-id="94a14-125">The example provided uses the BCP method.</span></span>
* <span data-ttu-id="94a14-126">[Ändra databasen](https://msdn.microsoft.com/library/bb522682.aspx) ändra schemat för transaktionsloggning till bulkloggad att minimera arbetet med att logga, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="94a14-126">[Alter the database](https://msdn.microsoft.com/library/bb522682.aspx) to change transaction logging scheme to BULK_LOGGED to minimize overhead of logging, e.g.:</span></span>
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* <span data-ttu-id="94a14-127">Starta import massåtgärder parallellt för att påskynda inläsning av data.</span><span class="sxs-lookup"><span data-stu-id="94a14-127">To expedite data loading, launch the bulk import operations in parallel.</span></span> <span data-ttu-id="94a14-128">Tips om snabbare bulk importerar av stordata i SQL Server-databaser finns [läsa in 1TB på mindre än 1 timme](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).</span><span class="sxs-lookup"><span data-stu-id="94a14-128">For tips on expediting bulk importing of big data into SQL Server databases, see [Load 1TB in less than 1 hour](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).</span></span>

<span data-ttu-id="94a14-129">Följande PowerShell-skript är ett exempel på parallella datainläsning med hjälp av BCP.</span><span class="sxs-lookup"><span data-stu-id="94a14-129">The following PowerShell script is an example of parallel data loading using BCP.</span></span>

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"

    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir

    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }

    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }

    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }

    Get-Job

    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a><span data-ttu-id="94a14-130">Skapa index för att optimera kopplingar och prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="94a14-130">Create indexes to optimize joins and query performance</span></span>
* <span data-ttu-id="94a14-131">Om du ska hämta data för modellering från flera tabeller, skapa index för koppling för att förbättra prestanda för kopplingen.</span><span class="sxs-lookup"><span data-stu-id="94a14-131">If you will extract data for modeling from multiple tables, create indexes on the join keys to improve the join performance.</span></span>
* <span data-ttu-id="94a14-132">[Skapa index](https://technet.microsoft.com/library/ms188783.aspx) (klustrade eller icke-grupperade) mål samma filgruppen för varje partition, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="94a14-132">[Create indexes](https://technet.microsoft.com/library/ms188783.aspx) (clustered or non-clustered) targeting the same filegroup for each partition, for e.g.:</span></span>
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  <span data-ttu-id="94a14-133">Eller,</span><span class="sxs-lookup"><span data-stu-id="94a14-133">or,</span></span>
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > <span data-ttu-id="94a14-134">Du kan välja att skapa index innan du importerar data samtidigt.</span><span class="sxs-lookup"><span data-stu-id="94a14-134">You may choose to create the indexes before bulk importing the data.</span></span> <span data-ttu-id="94a14-135">Skapandet av index innan massimport tar lång tid för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="94a14-135">Index creation before bulk importing will slow down the data loading.</span></span>
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a><span data-ttu-id="94a14-136">Processen för avancerade analyser och teknik i åtgärden exempel</span><span class="sxs-lookup"><span data-stu-id="94a14-136">Advanced Analytics Process and Technology in Action Example</span></span>
<span data-ttu-id="94a14-137">En slutpunkt till slutpunkt genomgången exempel med hjälp av Cortana Analytics Process med en offentlig dataset finns [Cortana Analytics processen i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="94a14-137">For an end-to-end walkthrough example using the Cortana Analytics Process with a public dataset, see [Cortana Analytics Process in Action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

