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
# <a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Kör Sqoop jobb med hjälp av .NET SDK för Hadoop i HDInsight
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Lär dig hur toouse HDInsight .NET SDK toorun Sqoop jobb i HDInsight tooimport och exportera mellan HDInsight-kluster och Azure SQL database eller SQL Server-databas.

> [!NOTE]
> hello steg i den här artikeln kan användas med antingen en Windows- eller Linux-baserade HDInsight-kluster; de här stegen fungerar dock bara från en Windows-klient. Använda hello flikväljaren överst hello av den här artikeln toochoose andra metoder.
> 
> 

### <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande objekt:

* **Ett Hadoop-kluster i HDInsight**. Se [skapa klustret och SQL database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-using-net-sdk"></a>Använda Sqoop på HDInsight-kluster med .NET SDK
Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET. I det här avsnittet skapar du en C#-konsolen programmet tooexport hello hivesampletable toohello SQL-databas tabell du skapade tidigare i den här kursen.

## <a name="submit-a-sqoop-job"></a>Skicka ett Sqoop-jobb

1. Skapa ett C#-konsolprogram i Visual Studio.
2. Kör följande Nuget-kommandot tooimport hello paketet hello från hello Visual Studio Package Manager-konsolen.
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Använd följande kod i filen Program.cs för hello hello:
   
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
4. Tryck på **F5** toorun hello program. 

## <a name="limitations"></a>Begränsningar
* Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.
* Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop utför flera infogningar i stället för batchbearbetning hello infogningsåtgärder.

## <a name="next-steps"></a>Nästa steg
Nu har du fått veta hur toouse Sqoop. Det finns fler toolearn:

* [Använda Oozie med HDInsight](hdinsight-use-oozie.md): Använd Sqoop åtgärd i ett arbetsflöde för Oozie.
* [Analysera svarta fördröjning data med HDInsight](hdinsight-analyze-flight-delay-data.md): använda Hive tooanalyze svarta fördröjning data och sedan använda Sqoop tooexport data tooan Azure SQL-databas.
* [Ladda upp data tooHDInsight](hdinsight-upload-data.md): hitta andra metoder för att överföra data tooHDInsight/Azure Blob storage.

