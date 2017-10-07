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
# <a name="move-data-toosql-server-on-an-azure-virtual-machine"></a><span data-ttu-id="4adfa-103">Flytta data tooSQL Server på en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="4adfa-103">Move data tooSQL Server on an Azure virtual machine</span></span>
<span data-ttu-id="4adfa-104">Det här avsnittet beskriver hello alternativ för att flytta data från flata filer (CSV eller TVS format) eller från en lokal SQL Server tooSQL Server på en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="4adfa-104">This topic outlines hello options for moving data either from flat files (CSV or TSV formats) or from an on-premises SQL Server tooSQL Server on an Azure virtual machine.</span></span> <span data-ttu-id="4adfa-105">Dessa uppgifter för glidande data toohello moln är en del av hello Team datavetenskap Process.</span><span class="sxs-lookup"><span data-stu-id="4adfa-105">These tasks for moving data toohello cloud are part of hello Team Data Science Process.</span></span>

<span data-ttu-id="4adfa-106">Ett avsnitt som visar hello alternativ för glidande data tooan Azure SQL Database för Machine Learning finns [flytta data tooan Azure SQL Database för Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4adfa-106">For a topic that outlines hello options for moving data tooan Azure SQL Database for Machine Learning, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

<span data-ttu-id="4adfa-107">Hej **menyn** nedan länkar tootopics som beskriver hur tooingest data till andra mål-miljöer där hello data kan lagras och behandlas under hello Team Data vetenskap processen (TDSP).</span><span class="sxs-lookup"><span data-stu-id="4adfa-107">hello **menu** below links tootopics that describe how tooingest data into other target environments where hello data can be stored and processed during hello Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="4adfa-108">hello sammanfattas följande tabell hello alternativ för att flytta data tooSQL Server på en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="4adfa-108">hello following table summarizes hello options for moving data tooSQL Server on an Azure virtual machine.</span></span>

| <span data-ttu-id="4adfa-109"><b>KÄLLA</b></span><span class="sxs-lookup"><span data-stu-id="4adfa-109"><b>SOURCE</b></span></span> | <span data-ttu-id="4adfa-110"><b>MÅL: SQLServer på Azure VM</b></span><span class="sxs-lookup"><span data-stu-id="4adfa-110"><b>DESTINATION: SQL Server on Azure VM</b></span></span> |
| --- | --- |
| <span data-ttu-id="4adfa-111"><b>Flat-fil</b></span><span class="sxs-lookup"><span data-stu-id="4adfa-111"><b>Flat File</b></span></span> |<span data-ttu-id="4adfa-112">1. <a href="#insert-tables-bcp">Kommandoradsverktyget bulk copy-verktyget (BCP)</a></span><span class="sxs-lookup"><span data-stu-id="4adfa-112">1. <a href="#insert-tables-bcp">Command-line bulk copy utility (BCP) </a></span></span><br> <span data-ttu-id="4adfa-113">2. <a href="#insert-tables-bulkquery">Bulk Insert SQL-fråga</a></span><span class="sxs-lookup"><span data-stu-id="4adfa-113">2. <a href="#insert-tables-bulkquery">Bulk Insert SQL Query </a></span></span><br> <span data-ttu-id="4adfa-114">3. <a href="#sql-builtin-utilities">Grafisk inbyggda verktyg i SQLServer</a></span><span class="sxs-lookup"><span data-stu-id="4adfa-114">3. <a href="#sql-builtin-utilities">Graphical Built-in Utilities in SQL Server</a></span></span> |
| <span data-ttu-id="4adfa-115"><b>Lokal SQLServer</b></span><span class="sxs-lookup"><span data-stu-id="4adfa-115"><b>On-Premises SQL Server</b></span></span> |<span data-ttu-id="4adfa-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Distribuera en SQL Server-databas tooa Microsoft Azure VM-guiden</a></span><span class="sxs-lookup"><span data-stu-id="4adfa-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</a></span></span><br> <span data-ttu-id="4adfa-117">2. <a href="#export-flat-file">Exportera tooa flat fil</a></span><span class="sxs-lookup"><span data-stu-id="4adfa-117">2. <a href="#export-flat-file">Export tooa flat File </a></span></span><br> <span data-ttu-id="4adfa-118">3. <a href="#sql-migration">Migreringsguiden för SQL-databas</a></span><span class="sxs-lookup"><span data-stu-id="4adfa-118">3. <a href="#sql-migration">SQL Database Migration Wizard </a></span></span> <br> <span data-ttu-id="4adfa-119">4. <a href="#sql-backup">Databasen tillbaka in och återställa</a></span><span class="sxs-lookup"><span data-stu-id="4adfa-119">4. <a href="#sql-backup">Database back up and restore </a></span></span><br> |

<span data-ttu-id="4adfa-120">Observera att det här dokumentet förutsätts att SQL-kommandon körs från SQL Server Management Studio eller Visual Studio Database Explorer.</span><span class="sxs-lookup"><span data-stu-id="4adfa-120">Note that this document assumes that SQL commands are executed from SQL Server Management Studio or Visual Studio Database Explorer.</span></span>

> [!TIP]
> <span data-ttu-id="4adfa-121">Alternativt kan du använda [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate och schemalägga en pipeline som flyttar data tooa SQL Server-VM på Azure.</span><span class="sxs-lookup"><span data-stu-id="4adfa-121">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate and schedule a pipeline that will move data tooa SQL Server VM on Azure.</span></span> <span data-ttu-id="4adfa-122">Mer information finns i [kopiera data med Azure Data Factory (Kopieringsaktiviteten)](../data-factory/data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="4adfa-122">For more information, see [Copy data with Azure Data Factory (Copy Activity)](../data-factory/data-factory-data-movement-activities.md).</span></span>
>
>

## <span data-ttu-id="4adfa-123"><a name="prereqs"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="4adfa-123"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="4adfa-124">Den här kursen förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="4adfa-124">This tutorial assumes you have:</span></span>

* <span data-ttu-id="4adfa-125">En **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="4adfa-125">An **Azure subscription**.</span></span> <span data-ttu-id="4adfa-126">Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4adfa-126">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4adfa-127">En **Azure storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="4adfa-127">An **Azure storage account**.</span></span> <span data-ttu-id="4adfa-128">Du använder ett Azure storage-konto för att lagra hello data i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="4adfa-128">You will use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="4adfa-129">Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel.</span><span class="sxs-lookup"><span data-stu-id="4adfa-129">If you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="4adfa-130">När du har skapat hello storage-konto behöver tooobtain hello konto nyckel som används för tooaccess hello lagring.</span><span class="sxs-lookup"><span data-stu-id="4adfa-130">After you have created hello storage account, you will need tooobtain hello account key used tooaccess hello storage.</span></span> <span data-ttu-id="4adfa-131">Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="4adfa-131">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="4adfa-132">Etablerad **SQLServer på en virtuell dator i Azure**.</span><span class="sxs-lookup"><span data-stu-id="4adfa-132">Provisioned **SQL Server on an Azure VM**.</span></span> <span data-ttu-id="4adfa-133">Instruktioner finns i [ställa in en Azure SQL Server-dator som en bärbar dator IPython server för avancerade analyser](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="4adfa-133">For instructions, see [Set up an Azure SQL Server virtual machine as an IPython Notebook server for advanced analytics](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span></span>
* <span data-ttu-id="4adfa-134">Installerat och konfigurerat **Azure PowerShell** lokalt.</span><span class="sxs-lookup"><span data-stu-id="4adfa-134">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="4adfa-135">Instruktioner finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4adfa-135">For instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="4adfa-136"><a name="filesource_to_sqlonazurevm"></a>Flytta data från en flat fil källa tooSQL Server på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="4adfa-136"><a name="filesource_to_sqlonazurevm"></a> Moving data from a flat file source tooSQL Server on an Azure VM</span></span>
<span data-ttu-id="4adfa-137">Om data finns i en flat-fil (ordnade i ett format för raden/kolumnen) kan vara det flyttade tooSQL serverns virtuella dator på Azure via hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="4adfa-137">If your data is in a flat file (arranged in a row/column format), it can be moved tooSQL Server VM on Azure via hello following methods:</span></span>

1. [<span data-ttu-id="4adfa-138">Kommandoradsverktyget bulk copy-verktyget (BCP)</span><span class="sxs-lookup"><span data-stu-id="4adfa-138">Command-line bulk copy utility (BCP)</span></span>](#insert-tables-bcp)
2. [<span data-ttu-id="4adfa-139">Bulk Insert SQL-fråga</span><span class="sxs-lookup"><span data-stu-id="4adfa-139">Bulk Insert SQL Query </span></span>](#insert-tables-bulkquery)
3. [<span data-ttu-id="4adfa-140">Grafisk inbyggda verktyg i SQLServer (importera och exportera, SSIS)</span><span class="sxs-lookup"><span data-stu-id="4adfa-140">Graphical Built-in Utilities in SQL Server (Import/Export, SSIS)</span></span>](#sql-builtin-utilities)

### <span data-ttu-id="4adfa-141"><a name="insert-tables-bcp"></a>Kommandoradsverktyget bulk copy-verktyget (BCP)</span><span class="sxs-lookup"><span data-stu-id="4adfa-141"><a name="insert-tables-bcp"></a>Command-line bulk copy utility (BCP)</span></span>
<span data-ttu-id="4adfa-142">BCP är ett kommandoradsverktyg som installerats med SQL Server och är en av hello snabbaste sätt toomove data.</span><span class="sxs-lookup"><span data-stu-id="4adfa-142">BCP is a command-line utility installed with SQL Server and is one of hello quickest ways toomove data.</span></span> <span data-ttu-id="4adfa-143">Fungerar för alla tre SQL Server-varianter (lokal SQLServer, SQL Azure och SQL Server-VM på Azure).</span><span class="sxs-lookup"><span data-stu-id="4adfa-143">It works across all three SQL Server variants (On-premises SQL Server, SQL Azure and SQL Server VM on Azure).</span></span>

> [!NOTE]
> <span data-ttu-id="4adfa-144">**Var ska Mina data för BCP?**</span><span class="sxs-lookup"><span data-stu-id="4adfa-144">**Where should my data be for BCP?**</span></span>  
> <span data-ttu-id="4adfa-145">När den inte är nödvändiga i filer som innehåller källdata finns på Överför hello samma dator som SQL Server-hello målservern möjliggör snabbare (hastighet vs lokal disk-i/o nätverkshastigheten).</span><span class="sxs-lookup"><span data-stu-id="4adfa-145">While it is not required, having files containing source data located on hello same machine as hello target SQL Server allows for faster transfers (network speed vs local disk IO speed).</span></span> <span data-ttu-id="4adfa-146">Du kan flytta hello flat-filer som innehåller data toohello datorn där SQL Server installeras med hjälp av olika filkopieringen verktyg som [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Lagringsutforskaren](http://storageexplorer.com/) eller windows kopiera och klistra in via fjärråtkomst Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="4adfa-146">You can move hello flat files containing data toohello machine where SQL Server is installed using various file copying tools such as [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) or windows copy/paste via Remote Desktop Protocol (RDP).</span></span>
>
>

1. <span data-ttu-id="4adfa-147">Se till att hello-databasen och hello tabeller skapas på hello måldatabasen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4adfa-147">Ensure that hello database and hello tables are created on hello target SQL Server database.</span></span> <span data-ttu-id="4adfa-148">Här är ett exempel på hur toodo som använder hello `Create Database` och `Create Table` kommandon:</span><span class="sxs-lookup"><span data-stu-id="4adfa-148">Here is an example of how toodo that using hello `Create Database` and `Create Table` commands:</span></span>

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. <span data-ttu-id="4adfa-149">Generera hello formatfil som beskriver hello schema för hello tabellen genom att utfärda hello följande kommando från hello kommandoradsverktyg för hello datorn där bcp är installerad.</span><span class="sxs-lookup"><span data-stu-id="4adfa-149">Generate hello format file that describes hello schema for hello table by issuing hello following command from hello command-line of hello machine where bcp is installed.</span></span>

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. <span data-ttu-id="4adfa-150">Infoga hello data i hello-databasen med hello bcp kommandot på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="4adfa-150">Insert hello data into hello database using hello bcp command as follows.</span></span> <span data-ttu-id="4adfa-151">Detta bör utgå från kommandoraden hello förutsatt att hello SQL Server är installerat på samma dator:</span><span class="sxs-lookup"><span data-stu-id="4adfa-151">This should work from hello command-line assuming that hello SQL Server is installed on same machine:</span></span>

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> <span data-ttu-id="4adfa-152">**Optimera BCP infogar** Se följande artikel hello ['Riktlinjer för hur du optimerar massimport'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) toooptimize sådana infogas.</span><span class="sxs-lookup"><span data-stu-id="4adfa-152">**Optimizing BCP Inserts** Please refer hello following article ['Guidelines for Optimizing Bulk Import'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) toooptimize such inserts.</span></span>
>
>

### <span data-ttu-id="4adfa-153"><a name="insert-tables-bulkquery-parallel"></a>Parallelizing infogningar för snabbare dataflyttning</span><span class="sxs-lookup"><span data-stu-id="4adfa-153"><a name="insert-tables-bulkquery-parallel"></a>Parallelizing Inserts for Faster Data Movement</span></span>
<span data-ttu-id="4adfa-154">Om hello data som du flyttar är stort, kan du påskynda processen genom att samtidigt köra flera BCP kommandon parallellt i ett PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="4adfa-154">If hello data you are moving is large, you can speed things up by simultaneously executing multiple BCP commands in parallel in a PowerShell Script.</span></span>

> [!NOTE]
> <span data-ttu-id="4adfa-155">**Stordata införandet** toooptimize datainläsning för stora och mycket stora datamängder, partitionera logiska och fysiska databastabeller med flera filgrupper och partition tabeller.</span><span class="sxs-lookup"><span data-stu-id="4adfa-155">**Big data Ingestion** toooptimize data loading for large and very large datasets, partition your logical and physical database tables using multiple filegroups and partition tables.</span></span> <span data-ttu-id="4adfa-156">Mer information om hur du skapar och läser in toopartition datatabeller finns [parallella belastningen SQL-partitionstabell](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="4adfa-156">For more information about creating and loading data toopartition tables, see [Parallel Load SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
>
>

<span data-ttu-id="4adfa-157">hello PowerShell-exempelskript nedan visar parallella infogningar med bcp:</span><span class="sxs-lookup"><span data-stu-id="4adfa-157">hello sample PowerShell script below demonstrate parallel inserts using bcp:</span></span>

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


### <span data-ttu-id="4adfa-158"><a name="insert-tables-bulkquery"></a>Bulk Insert SQL-fråga</span><span class="sxs-lookup"><span data-stu-id="4adfa-158"><a name="insert-tables-bulkquery"></a>Bulk Insert SQL Query</span></span>
<span data-ttu-id="4adfa-159">[Bulk Insert SQL-frågan](https://msdn.microsoft.com/library/ms188365) kan vara används tooimport data till hello-databas från raden/kolumnen baserat filer (hello stöds typer beskrivs i den[förbereder Data för Bulk exportera eller importera (SQL Server)](https://msdn.microsoft.com/library/ms188609)) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4adfa-159">[Bulk Insert SQL Query](https://msdn.microsoft.com/library/ms188365) can be used tooimport data into hello database from row/column based files (hello supported types are covered in the[Prepare Data for Bulk Export or Import (SQL Server)](https://msdn.microsoft.com/library/ms188609)) topic.</span></span>

<span data-ttu-id="4adfa-160">Här följer några exempelkommandon för Bulk Insert är enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="4adfa-160">Here are some sample commands for Bulk Insert are as below:</span></span>  

1. <span data-ttu-id="4adfa-161">Analysera dina data och ange anpassade alternativ innan du importerar toomake till SQL Server-databas som hello förutsätter hello samma format för eventuella särskilda fält, till exempel datum.</span><span class="sxs-lookup"><span data-stu-id="4adfa-161">Analyze your data and set any custom options before importing toomake sure that hello SQL Server database assumes hello same format for any special fields such as dates.</span></span> <span data-ttu-id="4adfa-162">Här är ett exempel på hur tooset hello datum format som år-månad-dag (om dina data innehåller hello datumet i formatet år-månad-dag):</span><span class="sxs-lookup"><span data-stu-id="4adfa-162">Here is an example of how tooset hello date format as year-month-day (if your data contains hello date in year-month-day format):</span></span>

        SET DATEFORMAT ymd;    
2. <span data-ttu-id="4adfa-163">Importera data med hjälp av bulk importuttryck:</span><span class="sxs-lookup"><span data-stu-id="4adfa-163">Import data using bulk import statements:</span></span>

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be hello row separator in your data
        )

### <span data-ttu-id="4adfa-164"><a name="sql-builtin-utilities"></a>Inbyggda verktyg i SQLServer</span><span class="sxs-lookup"><span data-stu-id="4adfa-164"><a name="sql-builtin-utilities"></a>Built-in Utilities in SQL Server</span></span>
<span data-ttu-id="4adfa-165">Du kan använda SQL Server integration Services (SSIS) tooimport data till SQL Server-VM på Azure från en flat-fil.</span><span class="sxs-lookup"><span data-stu-id="4adfa-165">You can use SQL Server Integrations Services (SSIS) tooimport data into SQL Server VM on Azure from a flat file.</span></span>
<span data-ttu-id="4adfa-166">SSIS finns i två studio miljöer.</span><span class="sxs-lookup"><span data-stu-id="4adfa-166">SSIS is available in two studio environments.</span></span> <span data-ttu-id="4adfa-167">Mer information finns i [Integration Services (SSIS) och Studio miljöer](https://technet.microsoft.com/library/ms140028.aspx):</span><span class="sxs-lookup"><span data-stu-id="4adfa-167">For details, see [Integration Services (SSIS) and Studio Environments](https://technet.microsoft.com/library/ms140028.aspx):</span></span>

* <span data-ttu-id="4adfa-168">Mer information om SQL Server Data Tools finns [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span><span class="sxs-lookup"><span data-stu-id="4adfa-168">For details on SQL Server Data Tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span></span>  
* <span data-ttu-id="4adfa-169">Mer information om hello guiden Importera och exportera finns [SQL Server importera och exportera](https://msdn.microsoft.com/library/ms141209.aspx)</span><span class="sxs-lookup"><span data-stu-id="4adfa-169">For details on hello Import/Export Wizard, see [SQL Server Import and Export Wizard](https://msdn.microsoft.com/library/ms141209.aspx)</span></span>

## <span data-ttu-id="4adfa-170"><a name="sqlonprem_to_sqlonazurevm"></a>Flytta Data från lokala SQL Server tooSQL Server på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="4adfa-170"><a name="sqlonprem_to_sqlonazurevm"></a>Moving Data from on-premises SQL Server tooSQL Server on an Azure VM</span></span>
<span data-ttu-id="4adfa-171">Du kan också använda följande övergångsstrategi hello:</span><span class="sxs-lookup"><span data-stu-id="4adfa-171">You can also use hello following migration strategies:</span></span>

1. [<span data-ttu-id="4adfa-172">Distribuera en SQL Server-databas tooa Microsoft Azure VM-guiden</span><span class="sxs-lookup"><span data-stu-id="4adfa-172">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</span></span>](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [<span data-ttu-id="4adfa-173">Exportera tooFlat fil</span><span class="sxs-lookup"><span data-stu-id="4adfa-173">Export tooFlat File</span></span>](#export-flat-file)
3. [<span data-ttu-id="4adfa-174">Migreringsguiden för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="4adfa-174">SQL Database Migration Wizard</span></span>](#sql-migration)
4. [<span data-ttu-id="4adfa-175">Databasen tillbaka in och återställa</span><span class="sxs-lookup"><span data-stu-id="4adfa-175">Database back up and restore</span></span>](#sql-backup)

<span data-ttu-id="4adfa-176">Vi beskrivs var och en av dessa nedan:</span><span class="sxs-lookup"><span data-stu-id="4adfa-176">We describe each of these below:</span></span>

### <a name="deploy-a-sql-server-database-tooa-microsoft-azure-vm-wizard"></a><span data-ttu-id="4adfa-177">Distribuera en SQL Server-databas tooa Microsoft Azure VM-guiden</span><span class="sxs-lookup"><span data-stu-id="4adfa-177">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</span></span>
<span data-ttu-id="4adfa-178">Hej **en SQL Server-databas tooa Microsoft Azure VM guiden distribuera** är ett enkelt och rekommenderade sätt toomove data från en lokal SQL Server-instansen tooSQL Server på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="4adfa-178">hello **Deploy a SQL Server Database tooa Microsoft Azure VM wizard** is a simple and recommended way toomove data from an on-premises SQL Server instance tooSQL Server on an Azure VM.</span></span> <span data-ttu-id="4adfa-179">För detaljerade anvisningar samt en beskrivning av andra alternativ, se [migrera en databas tooSQL Server på en Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span><span class="sxs-lookup"><span data-stu-id="4adfa-179">For detailed steps as well as a discussion of other alternatives, see [Migrate a database tooSQL Server on an Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span></span>

### <span data-ttu-id="4adfa-180"><a name="export-flat-file"></a>Exportera tooFlat fil</span><span class="sxs-lookup"><span data-stu-id="4adfa-180"><a name="export-flat-file"></a>Export tooFlat File</span></span>
<span data-ttu-id="4adfa-181">Olika metoder kan vara används toobulk exportera data från en lokal SQL Server enligt beskrivningen i hello [massimport och Export av Data (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4adfa-181">Various methods can be used toobulk export data from an On-Premises SQL Server as documented in hello [Bulk Import and Export of Data (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) topic.</span></span> <span data-ttu-id="4adfa-182">Det här dokumentet beskriver hello Bulk Copy Program (BCP) som ett exempel.</span><span class="sxs-lookup"><span data-stu-id="4adfa-182">This document will cover hello Bulk Copy Program (BCP) as an example.</span></span> <span data-ttu-id="4adfa-183">När data har exporterats till en flat-fil kan vara det importerade tooanother SQLServer med massimport.</span><span class="sxs-lookup"><span data-stu-id="4adfa-183">Once data is exported into a flat file, it can be imported tooanother SQL server using bulk import.</span></span>

1. <span data-ttu-id="4adfa-184">Exportera hello data från lokala SQL Server-tooa filen med hello bcp-verktyget på följande sätt</span><span class="sxs-lookup"><span data-stu-id="4adfa-184">Export hello data from on-premises SQL Server tooa File using hello bcp utility as follows</span></span>

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. <span data-ttu-id="4adfa-185">Skapa hello databas och hello tabellen på SQL Server-VM på Azure med hjälp av hello `create database` och `create table` för hello tabellschemat exporterade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="4adfa-185">Create hello database and hello table on SQL Server VM on Azure using hello `create database` and `create table` for hello table schema exported in step 1.</span></span>
3. <span data-ttu-id="4adfa-186">Skapa en fil för att beskriva hello tabellschemat hello data som exporteras eller importerade.</span><span class="sxs-lookup"><span data-stu-id="4adfa-186">Create a format file for describing hello table schema of hello data being exported/imported.</span></span> <span data-ttu-id="4adfa-187">Information om hello formatfilen beskrivs i [skapa formatfilen (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span><span class="sxs-lookup"><span data-stu-id="4adfa-187">Details of hello format file are described in [Create a Format File (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span></span>

    <span data-ttu-id="4adfa-188">Formatera Filgenerering när kör BCP från hello SQLServer-dator</span><span class="sxs-lookup"><span data-stu-id="4adfa-188">Format file generation when running BCP from hello SQL Server machine</span></span>

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    <span data-ttu-id="4adfa-189">Formatera Filgenerering när körs via fjärranslutning BCP mot en SQL Server</span><span class="sxs-lookup"><span data-stu-id="4adfa-189">Format file generation when running BCP remotely against a SQL Server</span></span>

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. <span data-ttu-id="4adfa-190">Använd någon av hello-metoder som beskrivs i avsnittet [flytta Data från filkälla](#filesource_to_sqlonazurevm) toomove hello data i flata filer tooa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4adfa-190">Use any of hello methods described in section [Moving Data from File Source](#filesource_to_sqlonazurevm) toomove hello data in flat files tooa SQL Server.</span></span>

### <span data-ttu-id="4adfa-191"><a name="sql-migration"></a>Migreringsguiden för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="4adfa-191"><a name="sql-migration"></a>SQL Database Migration Wizard</span></span>
<span data-ttu-id="4adfa-192">[SQL Server-databasen migreringsguiden](http://sqlazuremw.codeplex.com/) ger en användarvänlig sätt toomove data mellan två SQL server-instanser.</span><span class="sxs-lookup"><span data-stu-id="4adfa-192">[SQL Server Database Migration Wizard](http://sqlazuremw.codeplex.com/) provides a user-friendly way toomove data between two SQL server instances.</span></span> <span data-ttu-id="4adfa-193">Det tillåter hello toomap hello data användarschemat mellan datakällor och måltabell, Välj kolumntyper och andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="4adfa-193">It allows hello user toomap hello data schema between sources and destination tables, choose column types and various other functionalities.</span></span> <span data-ttu-id="4adfa-194">Masskopiering (BCP) används under hello omfattar.</span><span class="sxs-lookup"><span data-stu-id="4adfa-194">It uses bulk copy (BCP) under hello covers.</span></span> <span data-ttu-id="4adfa-195">En skärmbild av hello välkomstskärmen för hello SQL-Databasmigrering guiden visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4adfa-195">A screenshot of hello welcome screen for hello SQL Database Migration wizard is shown below.</span></span>  

![Guiden för SQL Server-migrering][2]

### <span data-ttu-id="4adfa-197"><a name="sql-backup"></a>Databasen tillbaka in och återställa</span><span class="sxs-lookup"><span data-stu-id="4adfa-197"><a name="sql-backup"></a>Database back up and restore</span></span>
<span data-ttu-id="4adfa-198">Har stöd för SQL Server:</span><span class="sxs-lookup"><span data-stu-id="4adfa-198">SQL Server supports:</span></span>

1. <span data-ttu-id="4adfa-199">[Databasen tillbaka in och återställa funktionen](https://msdn.microsoft.com/library/ms187048.aspx) (både tooa lokal fil eller bacpac export tooblob) och [Data nivåer program](https://msdn.microsoft.com/library/ee210546.aspx) (med bacpac).</span><span class="sxs-lookup"><span data-stu-id="4adfa-199">[Database back up and restore functionality](https://msdn.microsoft.com/library/ms187048.aspx) (both tooa local file or bacpac export tooblob) and [Data Tier Applications](https://msdn.microsoft.com/library/ee210546.aspx) (using bacpac).</span></span>
2. <span data-ttu-id="4adfa-200">Möjlighet toodirectly skapa virtuella SQL Server-datorer i Azure med en kopierade databas eller kopiera tooan befintliga SQL Azure-databas.</span><span class="sxs-lookup"><span data-stu-id="4adfa-200">Ability toodirectly create SQL Server VMs on Azure with a copied database or copy tooan existing SQL Azure database.</span></span> <span data-ttu-id="4adfa-201">Mer information finns i [Använd hello kopiera Databasguiden](https://msdn.microsoft.com/library/ms188664.aspx).</span><span class="sxs-lookup"><span data-stu-id="4adfa-201">For more details, see [Use hello Copy Database Wizard](https://msdn.microsoft.com/library/ms188664.aspx).</span></span>

<span data-ttu-id="4adfa-202">En skärmbild av hello databasen tillbaka upp/återställa alternativ från SQL Server Management Studio visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4adfa-202">A screenshot of hello Database back up/restore options from SQL Server Management Studio is shown below.</span></span>

![Importverktyget för SQL Server][1]

## <a name="resources"></a><span data-ttu-id="4adfa-204">Resurser</span><span class="sxs-lookup"><span data-stu-id="4adfa-204">Resources</span></span>
[<span data-ttu-id="4adfa-205">Migrera en databas tooSQL Server på en Azure VM</span><span class="sxs-lookup"><span data-stu-id="4adfa-205">Migrate a Database tooSQL Server on an Azure VM</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[<span data-ttu-id="4adfa-206">Översikt över SQL Server på Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="4adfa-206">SQL Server on Azure Virtual Machines overview</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
