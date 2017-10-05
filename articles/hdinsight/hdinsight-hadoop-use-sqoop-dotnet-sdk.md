---
title: "Kör Sqoop jobb med hjälp av .NET- och HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du använder HDInsight .NET SDK för att köra Sqoop importera och exportera mellan ett Hadoop-kluster och en Azure SQL database."
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
ms.openlocfilehash: c95641fc6d20e2911e007d1974b9e2c2398b3133
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="d6c17-104">Kör Sqoop jobb med hjälp av .NET SDK för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="d6c17-104">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="d6c17-105">Lär dig hur du använder HDInsight .NET SDK för att köra Sqoop jobb i HDInsight för att importera och exportera mellan HDInsight-kluster och Azure SQL database eller SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="d6c17-105">Learn how to use HDInsight .NET SDK to run Sqoop jobs in HDInsight to import and export between HDInsight cluster and Azure SQL database or SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="d6c17-106">Stegen i den här artikeln kan användas med antingen en Windows- eller Linux-baserade HDInsight-kluster; de här stegen fungerar dock bara från en Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="d6c17-106">The steps in this article can be used with either a Windows-based or Linux-based HDInsight cluster; however, these steps only work from a Windows client.</span></span> <span data-ttu-id="d6c17-107">Använd flikväljaren överst i den här artikeln för att välja andra metoder.</span><span class="sxs-lookup"><span data-stu-id="d6c17-107">Use the tab selector on the top of this article to choose other methods.</span></span>
> 
> 

### <a name="prerequisites"></a><span data-ttu-id="d6c17-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d6c17-108">Prerequisites</span></span>
<span data-ttu-id="d6c17-109">Innan du börjar den här självstudiekursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d6c17-109">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="d6c17-110">**Ett Hadoop-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d6c17-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="d6c17-111">Se [skapa klustret och SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span><span class="sxs-lookup"><span data-stu-id="d6c17-111">See [Create cluster and SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).</span></span>

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a><span data-ttu-id="d6c17-112">Använda Sqoop på HDInsight-kluster med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="d6c17-112">Use Sqoop on HDInsight clusters using .NET SDK</span></span>
<span data-ttu-id="d6c17-113">HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det lättare att arbeta med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="d6c17-113">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> <span data-ttu-id="d6c17-114">I det här avsnittet skapar du en C#-konsolprogram för att exportera hivesampletable till SQL Database-tabellen som du skapade tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d6c17-114">In this section, you create a C# console application to export the hivesampletable to the SQL Database table you created earlier in this tutorial.</span></span>

## <a name="submit-a-sqoop-job"></a><span data-ttu-id="d6c17-115">Skicka ett Sqoop-jobb</span><span class="sxs-lookup"><span data-stu-id="d6c17-115">Submit a Sqoop job</span></span>

1. <span data-ttu-id="d6c17-116">Skapa ett C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6c17-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="d6c17-117">Kör följande Nuget-kommando för att importera paketet från Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="d6c17-117">From the Visual Studio Package Manager Console, run the following Nuget command to import the package.</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="d6c17-118">Använd följande kod i filen Program.cs:</span><span class="sxs-lookup"><span data-stu-id="d6c17-118">Use the following code in the Program.cs file:</span></span>
   
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
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
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
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
4. <span data-ttu-id="d6c17-119">Tryck på **F5** att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="d6c17-119">Press **F5** to run the program.</span></span> 

## <a name="limitations"></a><span data-ttu-id="d6c17-120">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="d6c17-120">Limitations</span></span>
* <span data-ttu-id="d6c17-121">Massinläsning export - med Linux-baserat HDInsight, Sqoop-kopplingen används för att exportera data till Microsoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.</span><span class="sxs-lookup"><span data-stu-id="d6c17-121">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>
* <span data-ttu-id="d6c17-122">Batchbearbetning - med Linux-baserat HDInsight när du använder den `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning insert-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d6c17-122">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop performs multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6c17-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6c17-123">Next steps</span></span>
<span data-ttu-id="d6c17-124">Nu har du lärt dig hur du använder Sqoop.</span><span class="sxs-lookup"><span data-stu-id="d6c17-124">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="d6c17-125">Du kan läsa mer här:</span><span class="sxs-lookup"><span data-stu-id="d6c17-125">To learn more, see:</span></span>

* <span data-ttu-id="d6c17-126">[Använda Oozie med HDInsight](hdinsight-use-oozie.md): Använd Sqoop åtgärd i ett arbetsflöde för Oozie.</span><span class="sxs-lookup"><span data-stu-id="d6c17-126">[Use Oozie with HDInsight](hdinsight-use-oozie.md): Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="d6c17-127">[Analysera svarta fördröjning data med HDInsight](hdinsight-analyze-flight-delay-data.md): använda Hive att analysera svarta fördröjning data och sedan använda Sqoop för att exportera data till en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="d6c17-127">[Analyze flight delay data using HDInsight](hdinsight-analyze-flight-delay-data.md): Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="d6c17-128">[Överföra data till HDInsight](hdinsight-upload-data.md): hitta andra metoder för att överföra data till HDInsight-/ Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="d6c17-128">[Upload data to HDInsight](hdinsight-upload-data.md): Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

