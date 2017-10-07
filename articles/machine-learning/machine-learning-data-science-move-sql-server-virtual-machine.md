---
title: "aaaMove data tooSQL Server på en virtuell Azure-dator | Microsoft Docs"
description: "Flytta data från flata filer eller från en lokal SQL Server tooSQL Server på Azure VM."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 2c9ef1d3-4f5c-4b1f-bf06-223646c8af06
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 63e02158f9c99f4478c4eb1cde62c877983dcf27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-toosql-server-on-an-azure-virtual-machine"></a>Flytta data tooSQL Server på en virtuell Azure-dator
Det här avsnittet beskriver hello alternativ för att flytta data från flata filer (CSV eller TVS format) eller från en lokal SQL Server tooSQL Server på en virtuell Azure-dator. Dessa uppgifter för glidande data toohello moln är en del av hello Team datavetenskap Process.

Ett avsnitt som visar hello alternativ för glidande data tooan Azure SQL Database för Machine Learning finns [flytta data tooan Azure SQL Database för Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).

Hej **menyn** nedan länkar tootopics som beskriver hur tooingest data till andra mål-miljöer där hello data kan lagras och behandlas under hello Team Data vetenskap processen (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

hello sammanfattas följande tabell hello alternativ för att flytta data tooSQL Server på en virtuell Azure-dator.

| <b>KÄLLA</b> | <b>MÅL: SQLServer på Azure VM</b> |
| --- | --- |
| <b>Flat-fil</b> |1. <a href="#insert-tables-bcp">Kommandoradsverktyget bulk copy-verktyget (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">Bulk Insert SQL-fråga</a><br> 3. <a href="#sql-builtin-utilities">Grafisk inbyggda verktyg i SQLServer</a> |
| <b>Lokal SQLServer</b> |1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Distribuera en SQL Server-databas tooa Microsoft Azure VM-guiden</a><br> 2. <a href="#export-flat-file">Exportera tooa flat fil</a><br> 3. <a href="#sql-migration">Migreringsguiden för SQL-databas</a> <br> 4. <a href="#sql-backup">Databasen tillbaka in och återställa</a><br> |

Observera att det här dokumentet förutsätts att SQL-kommandon körs från SQL Server Management Studio eller Visual Studio Database Explorer.

> [!TIP]
> Alternativt kan du använda [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate och schemalägga en pipeline som flyttar data tooa SQL Server-VM på Azure. Mer information finns i [kopiera data med Azure Data Factory (Kopieringsaktiviteten)](../data-factory/data-factory-data-movement-activities.md).
>
>

## <a name="prereqs"></a>Förhandskrav
Den här kursen förutsätter att du har:

* En **Azure-prenumeration**. Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* En **Azure storage-konto**. Du använder ett Azure storage-konto för att lagra hello data i den här självstudiekursen. Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel. När du har skapat hello storage-konto behöver tooobtain hello konto nyckel som används för tooaccess hello lagring. Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Etablerad **SQLServer på en virtuell dator i Azure**. Instruktioner finns i [ställa in en Azure SQL Server-dator som en bärbar dator IPython server för avancerade analyser](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Installerat och konfigurerat **Azure PowerShell** lokalt. Instruktioner finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="filesource_to_sqlonazurevm"></a>Flytta data från en flat fil källa tooSQL Server på en Azure VM
Om data finns i en flat-fil (ordnade i ett format för raden/kolumnen) kan vara det flyttade tooSQL serverns virtuella dator på Azure via hello följande metoder:

1. [Kommandoradsverktyget bulk copy-verktyget (BCP)](#insert-tables-bcp)
2. [Bulk Insert SQL-fråga](#insert-tables-bulkquery)
3. [Grafisk inbyggda verktyg i SQLServer (importera och exportera, SSIS)](#sql-builtin-utilities)

### <a name="insert-tables-bcp"></a>Kommandoradsverktyget bulk copy-verktyget (BCP)
BCP är ett kommandoradsverktyg som installerats med SQL Server och är en av hello snabbaste sätt toomove data. Fungerar för alla tre SQL Server-varianter (lokal SQLServer, SQL Azure och SQL Server-VM på Azure).

> [!NOTE]
> **Var ska Mina data för BCP?**  
> När den inte är nödvändiga i filer som innehåller källdata finns på Överför hello samma dator som SQL Server-hello målservern möjliggör snabbare (hastighet vs lokal disk-i/o nätverkshastigheten). Du kan flytta hello flat-filer som innehåller data toohello datorn där SQL Server installeras med hjälp av olika filkopieringen verktyg som [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Lagringsutforskaren](http://storageexplorer.com/) eller windows kopiera och klistra in via fjärråtkomst Desktop Protocol (RDP).
>
>

1. Se till att hello-databasen och hello tabeller skapas på hello måldatabasen för SQL Server. Här är ett exempel på hur toodo som använder hello `Create Database` och `Create Table` kommandon:

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. Generera hello formatfil som beskriver hello schema för hello tabellen genom att utfärda hello följande kommando från hello kommandoradsverktyg för hello datorn där bcp är installerad.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. Infoga hello data i hello-databasen med hello bcp kommandot på följande sätt. Detta bör utgå från kommandoraden hello förutsatt att hello SQL Server är installerat på samma dator:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optimera BCP infogar** Se följande artikel hello ['Riktlinjer för hur du optimerar massimport'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) toooptimize sådana infogas.
>
>

### <a name="insert-tables-bulkquery-parallel"></a>Parallelizing infogningar för snabbare dataflyttning
Om hello data som du flyttar är stort, kan du påskynda processen genom att samtidigt köra flera BCP kommandon parallellt i ett PowerShell-skript.

> [!NOTE]
> **Stordata införandet** toooptimize datainläsning för stora och mycket stora datamängder, partitionera logiska och fysiska databastabeller med flera filgrupper och partition tabeller. Mer information om hur du skapar och läser in toopartition datatabeller finns [parallella belastningen SQL-partitionstabell](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).
>
>

hello PowerShell-exempelskript nedan visar parallella infogningar med bcp:

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for hello script tooexecute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n
      }


    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }


    # Wait for it all toocomplete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting hello information back from hello jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset hello execution policy


### <a name="insert-tables-bulkquery"></a>Bulk Insert SQL-fråga
[Bulk Insert SQL-frågan](https://msdn.microsoft.com/library/ms188365) kan vara används tooimport data till hello-databas från raden/kolumnen baserat filer (hello stöds typer beskrivs i den[förbereder Data för Bulk exportera eller importera (SQL Server)](https://msdn.microsoft.com/library/ms188609)) avsnittet.

Här följer några exempelkommandon för Bulk Insert är enligt nedan:  

1. Analysera dina data och ange anpassade alternativ innan du importerar toomake till SQL Server-databas som hello förutsätter hello samma format för eventuella särskilda fält, till exempel datum. Här är ett exempel på hur tooset hello datum format som år-månad-dag (om dina data innehåller hello datumet i formatet år-månad-dag):

        SET DATEFORMAT ymd;    
2. Importera data med hjälp av bulk importuttryck:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be hello row separator in your data
        )

### <a name="sql-builtin-utilities"></a>Inbyggda verktyg i SQLServer
Du kan använda SQL Server integration Services (SSIS) tooimport data till SQL Server-VM på Azure från en flat-fil.
SSIS finns i två studio miljöer. Mer information finns i [Integration Services (SSIS) och Studio miljöer](https://technet.microsoft.com/library/ms140028.aspx):

* Mer information om SQL Server Data Tools finns [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
* Mer information om hello guiden Importera och exportera finns [SQL Server importera och exportera](https://msdn.microsoft.com/library/ms141209.aspx)

## <a name="sqlonprem_to_sqlonazurevm"></a>Flytta Data från lokala SQL Server tooSQL Server på en Azure VM
Du kan också använda följande övergångsstrategi hello:

1. [Distribuera en SQL Server-databas tooa Microsoft Azure VM-guiden](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Exportera tooFlat fil](#export-flat-file)
3. [Migreringsguiden för SQL-databas](#sql-migration)
4. [Databasen tillbaka in och återställa](#sql-backup)

Vi beskrivs var och en av dessa nedan:

### <a name="deploy-a-sql-server-database-tooa-microsoft-azure-vm-wizard"></a>Distribuera en SQL Server-databas tooa Microsoft Azure VM-guiden
Hej **en SQL Server-databas tooa Microsoft Azure VM guiden distribuera** är ett enkelt och rekommenderade sätt toomove data från en lokal SQL Server-instansen tooSQL Server på en virtuell dator i Azure. För detaljerade anvisningar samt en beskrivning av andra alternativ, se [migrera en databas tooSQL Server på en Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Exportera tooFlat fil
Olika metoder kan vara används toobulk exportera data från en lokal SQL Server enligt beskrivningen i hello [massimport och Export av Data (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) avsnittet. Det här dokumentet beskriver hello Bulk Copy Program (BCP) som ett exempel. När data har exporterats till en flat-fil kan vara det importerade tooanother SQLServer med massimport.

1. Exportera hello data från lokala SQL Server-tooa filen med hello bcp-verktyget på följande sätt

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. Skapa hello databas och hello tabellen på SQL Server-VM på Azure med hjälp av hello `create database` och `create table` för hello tabellschemat exporterade i steg 1.
3. Skapa en fil för att beskriva hello tabellschemat hello data som exporteras eller importerade. Information om hello formatfilen beskrivs i [skapa formatfilen (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Formatera Filgenerering när kör BCP från hello SQLServer-dator

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    Formatera Filgenerering när körs via fjärranslutning BCP mot en SQL Server

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. Använd någon av hello-metoder som beskrivs i avsnittet [flytta Data från filkälla](#filesource_to_sqlonazurevm) toomove hello data i flata filer tooa SQL Server.

### <a name="sql-migration"></a>Migreringsguiden för SQL-databas
[SQL Server-databasen migreringsguiden](http://sqlazuremw.codeplex.com/) ger en användarvänlig sätt toomove data mellan två SQL server-instanser. Det tillåter hello toomap hello data användarschemat mellan datakällor och måltabell, Välj kolumntyper och andra funktioner. Masskopiering (BCP) används under hello omfattar. En skärmbild av hello välkomstskärmen för hello SQL-Databasmigrering guiden visas nedan.  

![Guiden för SQL Server-migrering][2]

### <a name="sql-backup"></a>Databasen tillbaka in och återställa
Har stöd för SQL Server:

1. [Databasen tillbaka in och återställa funktionen](https://msdn.microsoft.com/library/ms187048.aspx) (både tooa lokal fil eller bacpac export tooblob) och [Data nivåer program](https://msdn.microsoft.com/library/ee210546.aspx) (med bacpac).
2. Möjlighet toodirectly skapa virtuella SQL Server-datorer i Azure med en kopierade databas eller kopiera tooan befintliga SQL Azure-databas. Mer information finns i [Använd hello kopiera Databasguiden](https://msdn.microsoft.com/library/ms188664.aspx).

En skärmbild av hello databasen tillbaka upp/återställa alternativ från SQL Server Management Studio visas nedan.

![Importverktyget för SQL Server][1]

## <a name="resources"></a>Resurser
[Migrera en databas tooSQL Server på en Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[Översikt över SQL Server på Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
