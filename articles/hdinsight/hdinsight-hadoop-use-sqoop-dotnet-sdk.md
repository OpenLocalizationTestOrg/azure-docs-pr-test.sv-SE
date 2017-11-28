---
title: "aaaRun Sqoop jobb med hjälp av .NET- och HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse HDInsight .NET SDK toorun Sqoop importera och exportera mellan ett Hadoop-kluster och en Azure SQL database."
keywords: sqoop jobb
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 87bacd13-7775-4b71-91da-161cb6224a96
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: afa0a78ba5e5d89c04ba7be4b58dd24aea4f39ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="99ea2-104">Kör Sqoop jobb med hjälp av .NET SDK för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="99ea2-104">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="99ea2-105">Lär dig hur toouse HDInsight .NET SDK toorun Sqoop jobb i HDInsight tooimport och exportera mellan HDInsight-kluster och Azure SQL database eller SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="99ea2-105">Learn how toouse HDInsight .NET SDK toorun Sqoop jobs in HDInsight tooimport and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="99ea2-106">hello steg i den här artikeln kan användas med antingen en Windows- eller Linux-baserade HDInsight-kluster; de här stegen fungerar dock bara från en Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="99ea2-106">hello steps in this article can be used with either a Windows-based or Linux-based HDInsight cluster; however, these steps only work from a Windows client.</span></span> <span data-ttu-id="99ea2-107">Använda hello flikväljaren överst hello av den här artikeln toochoose andra metoder.</span><span class="sxs-lookup"><span data-stu-id="99ea2-107">Use hello tab selector on hello top of this article toochoose other methods.</span></span>
> 
> 

### <a name="prerequisites"></a><span data-ttu-id="99ea2-108">Krav</span><span class="sxs-lookup"><span data-stu-id="99ea2-108">Prerequisites</span></span>
<span data-ttu-id="99ea2-109">Innan du påbörjar den här självstudien måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="99ea2-109">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="99ea2-110">**Ett Hadoop-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="99ea2-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="99ea2-111">Se [skapa klustret och SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span><span class="sxs-lookup"><span data-stu-id="99ea2-111">See [Create cluster and SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span></span>

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a><span data-ttu-id="99ea2-112">Använda Sqoop på HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="99ea2-112">Use Sqoop on HDInsight clusters using .NET SDK</span></span>
<span data-ttu-id="99ea2-113">Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="99ea2-113">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span> <span data-ttu-id="99ea2-114">I det här avsnittet skapar du en C#-konsolen programmet tooexport hello hivesampletable toohello SQL-databas tabell du skapade tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="99ea2-114">In this section, you create a C# console application tooexport hello hivesampletable toohello SQL Database table you created earlier in this tutorial.</span></span>

## <a name="submit-a-sqoop-job"></a><span data-ttu-id="99ea2-115">Skicka ett Sqoop-jobb</span><span class="sxs-lookup"><span data-stu-id="99ea2-115">Submit a Sqoop job</span></span>

1. <span data-ttu-id="99ea2-116">Skapa ett C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99ea2-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="99ea2-117">Kör följande Nuget-kommandot tooimport hello paketet hello från hello Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="99ea2-117">From hello Visual Studio Package Manager Console, run hello following Nuget command tooimport hello package.</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="99ea2-118">Använd följande kod i filen Program.cs för hello hello:</span><span class="sxs-lookup"><span data-stu-id="99ea2-118">Use hello following code in hello Program.cs file:</span></span>
   
        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting hello Sqoop job toohello cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that hello response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating hello response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
4. <span data-ttu-id="99ea2-119">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="99ea2-119">Press **F5** toorun hello program.</span></span> 

## <a name="limitations"></a><span data-ttu-id="99ea2-120">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="99ea2-120">Limitations</span></span>
* <span data-ttu-id="99ea2-121">Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.</span><span class="sxs-lookup"><span data-stu-id="99ea2-121">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="99ea2-122">Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning hello infogningsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="99ea2-122">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99ea2-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99ea2-123">Next steps</span></span>
<span data-ttu-id="99ea2-124">Nu har du fått veta hur toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="99ea2-124">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="99ea2-125">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="99ea2-125">toolearn more, see:</span></span>

* <span data-ttu-id="99ea2-126">[Använda Oozie med HDInsight](hdinsight-use-oozie.md): Använd Sqoop åtgärd i ett arbetsflöde för Oozie.</span><span class="sxs-lookup"><span data-stu-id="99ea2-126">[Use Oozie with HDInsight](hdinsight-use-oozie.md): Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="99ea2-127">[Analysera svarta fördröjning data med HDInsight](hdinsight-analyze-flight-delay-data.md): använda Hive tooanalyze svarta fördröjning data och sedan använda Sqoop tooexport data tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="99ea2-127">[Analyze flight delay data using HDInsight](hdinsight-analyze-flight-delay-data.md): Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="99ea2-128">[Ladda upp data tooHDInsight](hdinsight-upload-data.md): hitta andra metoder för att överföra data tooHDInsight/Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="99ea2-128">[Upload data tooHDInsight](hdinsight-upload-data.md): Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

