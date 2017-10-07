---
title: "aaaBuild och optimera tabeller för snabb parallella import av data till en SQL Server på en virtuell dator i Azure | Microsoft Docs"
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
ms.openlocfilehash: ab748c47348ec6ca3b98ba39e27181bba5d36fc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Parallell massimport av data med SQL-tabeller för partition
Det här dokumentet beskriver hur toobuild partitionerade tabeller för snabb parallella massimport av data tooa SQL Server-databas. För stordata inläsning/överföring tooa SQL-databas importerar data toohello SQL DB och efterföljande frågor kan förbättras genom att använda *partitionerade tabeller och vyer*. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Skapa en ny databas och en uppsättning filgrupper
* [Skapa en ny databas](https://technet.microsoft.com/library/ms176061.aspx), om den inte redan finns.
* Lägg till filgrupper toohello databasen som innehåller hello partitionerad fysiska filer. Detta kan göras med [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) om nya eller [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) om hello databas finns redan.
* Lägg till en eller flera filer (om det behövs) tooeach databasen filgruppen.
  
  > [!NOTE]
  > Ange hello mål filgrupp som lagrar data för den här partitionen och hello fysiska databasen filnamn där hello filgruppsdata ska lagras.
  > 
  > 

hello följande exempel skapas en ny databas med tre filgrupper än hello primära och loggrupper, som innehåller en fysisk fil i varje. hello-databasfilerna skapas i hello standardmapp till SQL Server-Data, som konfigurerats i hello SQL Server-instansen. Läs mer om hello standardsökvägar [filplatser för standard- och namngivna instanser av SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

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

## <a name="create-a-partitioned-table"></a>Skapa en partitionerad tabell
Skapa partitionerade tabeller enligt toohello dataschemat, mappade toohello databasen filgrupper skapade i föregående steg i hello. När data importeras bulk toohello partitionerade tabeller, poster fördelas mellan hello filgrupper enligt tooa partitionsschema som beskrivs nedan.

**toocreate en partitionstabell, måste du:**

* [Skapa en partitionsfunktion](https://msdn.microsoft.com/library/ms187802.aspx) som definierar hello intervall med värden/gränser toobe ingår i varje enskild partitionstabell, t.ex. toolimit partitioner per månad (vissa\_datetime\_fältet) hello år 2013:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )
* [Skapa ett partitionsschema](https://msdn.microsoft.com/library/ms179854.aspx) som mappar varje partitionsintervall i hello partition funktionen tooa fysiska filgruppen, t.ex.:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> too(
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  tooverify hello adressintervall i praktiken i varje partitions bl.a toohello funktionen schemat, kör följande fråga hello:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>
* [Skapa en partitionerad tabell](https://msdn.microsoft.com/library/ms174979.aspx)(s) enligt tooyour dataschemat och ange hello partition schema och begränsning fältet används toopartition hello tabell, t.ex.:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Mer information finns i [skapa partitionerade tabeller och index](https://msdn.microsoft.com/library/ms188730.aspx).

## <a name="bulk-import-hello-data-for-each-individual-partition-table"></a>Massinläsning importera hello data för varje enskild partitionstabellen
* Du kan använda BCP BULK INSERT eller andra metoder som [Migreringsguiden för SQL Server](http://sqlazuremw.codeplex.com/). hello exempel använder hello BCP-metod.
* [Ändra hello databasen](https://msdn.microsoft.com/library/bb522682.aspx) toochange transaktion loggning schemat tooBULK_LOGGED toominimize arbetet med att logga, t.ex.:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* tooexpedite datainläsning, starta hello importera massåtgärder parallellt. Tips om snabbare bulk importerar av stordata i SQL Server-databaser finns [läsa in 1TB på mindre än 1 timme](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

hello är följande PowerShell-skript ett exempel på parallella datainläsning med hjälp av BCP.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # hello example assumes hello partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes hello input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set hello server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match hello number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name toobe loaded, basename of input data files, input format file, and number of partitions
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


## <a name="create-indexes-toooptimize-joins-and-query-performance"></a>Skapa index toooptimize kopplingar och prestanda för frågor
* Om du ska hämta data för modellering från flera tabeller, skapa index för hello koppling för tooimprove hello koppling prestanda.
* [Skapa index](https://technet.microsoft.com/library/ms188783.aspx) (klustrade eller icke-grupperade) riktad hello samma filgrupp för varje partition för t.ex.:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  Eller,
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > Du kan välja toocreate hello index innan bulk importerar hello data. Skapandet av index innan massimport kommer långsammare hello datainläsning.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Processen för avancerade analyser och teknik i åtgärden exempel
En slutpunkt till slutpunkt genomgången exempel med en offentlig dataset hello Cortana Analytics Process finns [Cortana Analytics processen i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

