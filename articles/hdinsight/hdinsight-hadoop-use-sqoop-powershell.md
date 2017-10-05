---
title: "Kör jobb för Sqoop med PowerShell och Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder Azure PowerShell från en arbetsstation för att köra Sqoop importera och exportera mellan ett Hadoop-kluster och en Azure SQL database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: bbb6f53a-e019-4d01-92bd-92c208c760b6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 956f4ac7c39e2936a2a6b5e5108dbe302446270c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-sqoop-jobs-using-azure-powershell-for-hadoop-in-hdinsight"></a><span data-ttu-id="e7466-103">Kör Sqoop jobb med hjälp av Azure PowerShell för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7466-103">Run Sqoop jobs using Azure PowerShell for Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="e7466-104">Lär dig hur du kör Sqoop jobb i HDInsight för att importera och exportera mellan HDInsight-kluster och Azure SQL database eller SQL Server-databas med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e7466-104">Learn how to use Azure PowerShell to run Sqoop jobs in HDInsight to import and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="e7466-105">Stegen i den här artikeln kan användas med antingen en Windows- eller Linux-baserade HDInsight-kluster; de här stegen fungerar bara i en windowsklient.</span><span class="sxs-lookup"><span data-stu-id="e7466-105">The steps in this article can be used with either a Windows-based or Linux-based HDInsight cluster; however, these steps will only work from a Windows client.</span></span> <span data-ttu-id="e7466-106">Andra metoder för överföring av jobbet, klickar du på flikväljaren överst i artikeln.</span><span class="sxs-lookup"><span data-stu-id="e7466-106">For other job submission methods, click the tab selector on the top of the article.</span></span>
> 
> 

### <a name="prerequisites"></a><span data-ttu-id="e7466-107">Krav</span><span class="sxs-lookup"><span data-stu-id="e7466-107">Prerequisites</span></span>
<span data-ttu-id="e7466-108">Innan du påbörjar de här självstudierna måste du ha:</span><span class="sxs-lookup"><span data-stu-id="e7466-108">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="e7466-109">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="e7466-109">**A workstation with Azure PowerShell**.</span></span>
  
    [!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
* <span data-ttu-id="e7466-110">**Ett Hadoop-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e7466-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="e7466-111">Se [skapa klustret och SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span><span class="sxs-lookup"><span data-stu-id="e7466-111">See [Create cluster and SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span></span>

## <a name="run-sqoop-using-powershell"></a><span data-ttu-id="e7466-112">Kör Sqoop med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="e7466-112">Run Sqoop using PowerShell</span></span>
<span data-ttu-id="e7466-113">Följande PowerShell-skript före bearbetar källfilen och exporterar den till en Azure SQL database:</span><span class="sxs-lookup"><span data-stu-id="e7466-113">The following PowerShell script pre-processes the source file, and exports it to an Azure SQL database:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $hdinsightClusterName = "<HDInsightClusterName>"

    $httpUserName = "admin"
    $httpPassword = "<Password>"

    $defaultStorageAccountName = $hdinsightClusterName + "store"
    $defaultBlobContainerName = $hdinsightClusterName


    $sqlDatabaseServerName = $hdinsightClusterName + "dbserver"
    $sqlDatabaseName = $hdinsightClusterName + "db"
    $sqlDatabaseLogin = "sqluser"
    $sqlDatabasePassword = "<Password>"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - pre-process the source file

    Write-Host "`nPreprocessing the source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define the connection string
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing the source and destination blob.
    $storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $defaultStorageAccountName
    $storageContainer = ($storageAccount |Get-AzureStorageContainer -Name $defaultBlobContainerName).CloudBlobContainer
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export the log file from the cluster to the SQL database

    Write-Host "Exporting the log file ..." -ForegroundColor Green

    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $tableName_log4j = "log4jlogs"
    $exportDir_log4j = "/tutorials/usesqoop/data"
    $sqljdbcdriver = "/user/oozie/share/lib/sqoop/sqljdbc41.jar"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1" `
        -Files $sqljdbcdriver

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    #endregion

## <a name="limitations"></a><span data-ttu-id="e7466-114">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="e7466-114">Limitations</span></span>
* <span data-ttu-id="e7466-115">Massinläsning export - med Linux-baserat HDInsight, Sqoop-kopplingen används för att exportera data till Microsoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.</span><span class="sxs-lookup"><span data-stu-id="e7466-115">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="e7466-116">Batchbearbetning - med Linux-baserat HDInsight när du använder den `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning insert-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e7466-116">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop will perform multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7466-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7466-117">Next steps</span></span>
<span data-ttu-id="e7466-118">Nu har du lärt dig hur du använder Sqoop.</span><span class="sxs-lookup"><span data-stu-id="e7466-118">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="e7466-119">Du kan läsa mer här:</span><span class="sxs-lookup"><span data-stu-id="e7466-119">To learn more, see:</span></span>

* <span data-ttu-id="e7466-120">[Använda Oozie med HDInsight](hdinsight-use-oozie.md): Använd Sqoop åtgärd i ett arbetsflöde för Oozie.</span><span class="sxs-lookup"><span data-stu-id="e7466-120">[Use Oozie with HDInsight](hdinsight-use-oozie.md): Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="e7466-121">[Analysera svarta fördröjning data med HDInsight](hdinsight-analyze-flight-delay-data.md): använda Hive att analysera svarta fördröjning data och sedan använda Sqoop för att exportera data till en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="e7466-121">[Analyze flight delay data using HDInsight](hdinsight-analyze-flight-delay-data.md): Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="e7466-122">[Överföra data till HDInsight](hdinsight-upload-data.md): hitta andra metoder för att överföra data till HDInsight-/ Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="e7466-122">[Upload data to HDInsight](hdinsight-upload-data.md): Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
